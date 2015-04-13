This page is intended to model threats associated with a basic Direct Project conversation between two email clients, each using a "full service" HISP that provides mail server functionality, server-side security agent processing and DNS services. Although messages may contain multiple recipients, we have constrained this model to only one for simplicity. There are of course a number of other possible configurations that alter the threat profile; these will be evaluated on separate pages as appropriate.  

 This threat assessment followed the [Threat Model Process](/Threat+Model+Process). Other Deployment Models have been similarly assessed and are cataloged on the [Threat Models](/Threat+Models) page.  

### Pre-conditions

*   All normal Direct Project [Consensus Proposal - Assumptions](/Consensus+Proposal) are in play.
*   The sender and receiver have ensured that agents of the sender and receiver (for example, HIO, HISP, intermediary) are authorized to act as such and are authorized to handle protected health information according to law and policy.

### Benefits

*   Since the e-mail client does not need to perform S/MIME functions, any e-mail client can be used
*   The HISP has access to all the content so could provide value added services, such as informing Account of Disclosures
*   The management of certificates and private keys is offloaded to the HISP, minimizing the burden on the client's implementation

### Drawbacks

*   TLS is mandated between Client and HISP to assure confidentiality of information transmitted
*   HISP has full access to PHI
*   HISP has full access to Private key, thus can impersonate the user

### Note on MDN "Read Receipts"

 The Direct Project specification formalizes the use of the Message Disposition Notification ("MDN") mechanism for confirming receipt of a message. Because these notifications are formatted and transmitted as standard Direct Project messages using encryption and signatures, their threat profile is equivalent to other messages. For this reason, the MDN mechanism is not separately addressed in this model.  

# Diagram:

 The following diagram depicts the key components and arcs involved in this conversation:  
 ![NHIN-D_SMTP_Threat_Model_SPN_20100624.jpg](/file/view/NHIN-D_SMTP_Threat_Model_SPN_20100624.jpg/161396195/NHIN-D_SMTP_Threat_Model_SPN_20100624.jpg "NHIN-D_SMTP_Threat_Model_SPN_20100624.jpg")  

# Arc 1 - Source Client sends message to Source SMTP Server

 In this arc, the source client first establishes an encrypted and authenticated SMTP connection to its configured mail server. The client then sends a standard email message over this connection. The message is received by the mail server as clear text.  

# Arc 2 - Source Agent fetches locally-configured private keys and anchor certificates

 In this arc, the agent on the server queries a local configuration resource such as a database, file system or certificate store for the private keys and anchor certificates associated with the source address. Because this arc occurs completely within the trusted boundaries of the HISP, it is expected that the agent will be able to securely make this request using standard secure programming practices. We will not further model this arc in this document.  

# Arcs 3 & 4 - Source Agent fetches and verifies public certificates for destination address

 In these arcs, the source agent uses DNS to fetch the public certificates (as CERT records) corresponding to the destination address, to be used to encrypt the message. The agent iterates over each returned certificate, attempting to find one that matches the validation criteria (subject matches address or address domain; not expired; not revoked) and has an anchor certificate in its issuing chain. As the issuing chain hierarchy is explored, any intermediate certificates will also be fetched using DNS and validated using the same criteria. Of course, configured anchor certificates will NOT be fetched in this manner; they must be manually configured.  

 In Arc 3.1, the agent makes DNS CERT requests to its configured DNS servers.  

 In Arc 3.2, the configured DNS server forwards non-local CERT requests through the distributed public DNS hierarchy. For the destination address CERT records, this will ultimately result in a request to the destination DNS server unless they have been cached at some DNS server in the search path. For intermediate issuing chain CERT records, the final DNS server is the one responsible for that organization's certificates.  

 In Arc 4, the CRL or OCSP service as identified in found certificates is queried for revocation status. The results of these queries will likely be locally-cached for performance.  

# Arc 5 - Source SMTP Server queues message for send

 In this arc, the encrypted message is placed in a local queue to be picked up and sent to the destination SMTP server. While typically the lifetime of a message in this queue will be very short, in cases where the queue becomes overburdened or the destination SMTP server is temporarily unreachable that lifetime could be signficant.  

# Arc 6 - Source SMTP Server resolves Destination SMTP Server addresses

 In Arc 6.1, the SMTP server queries its locally-configured DNS server for the MX records corresponding to the domain name of the destination address. In Arc 6.2, the configured DNS server forwards non-local MX requests through the distributed public DNS hierarchy.  

# Arc 7 - Source SMTP Server sends encrypted message to Destination SMTP Server

 In this arc, the source SMTP server establishes an encrypted (using TLS with a server certificate issued by a generally-accepted Internet CA) but unauthenticated SMTP connection to the destination SMTP server. The encrypted message is sent over this channel using standard SMTP mechanisms.  

# Arc 8 - Destination Agent fetches locally-configured private keys and anchor certificates

 In this arc, the agent on the server queries a local configuration resource such as a database, file system or certificate store for the private keys and anchor certificates associated with the destination address. Because this arc occurs completely within the trusted boundaries of the HISP, it is expected that the agent will be able to securely make this request using standard secure programming practices. We will not further model this arc in this document.  

# Arcs 9 & 10 - Destination Agent fetches and verifies public certificates for source address

 Signature validation is done using the certificates embedded in the message itself, so these certificates do not need to be fetched using DNS. However, as the agent explores the certificates and issuing chain hierarchy searching for a configured anchor certificate, intermediate certificates may need to be fetched and in all cases revocation status must be determined.  

 Arcs 9.1 and 9.2 are the equivalent of Arcs 3.1 and 3.2 for intermediate certificates found in issuing chains. Arc 10 is the equivalent of Arc 4 for all certificates interrogated.  

 Because these risks are the same, they are not duplicated here. However, it is worth noting that the denial-of-service impacts in these arcs fall on senders whose messages will be rejected by recipients. As with other DOS mitigations in this model, system monitoring systems can detect that a large number of rejections warrants further investigation.  

# Arc 11 - Destination SMTP Server stores clear text message for pickup

 In this arc, the message has been accepted for delivery to the destination address and must be stored in the address' assigned mailbox pending pickup and deletion by the destination client.  

# Arc 12 - Destination Client fetches message from Destination SMTP Server

 In this arc, the destination client first establishes an encrypted (using TLS with a server certificate issued by a generally-accepted Internet CA) and authenticated (using some means as negotiated for the particular edge protocol) POP3 or IMAP4 connection to its configured mail server. The client then sends standard requests to receive message headers and contents over this connection. The messages are received by the destination client as clear text.  

# Risk Assessment and Mitigation Plan

 In the interests of keeping scope on the Risk Assessment: We assume good programming practices and good business operational practices. For example we do not include risks of poor protocol implementation that would expose buffer-overflows, or poor operational environment that allows sharing of accounts, key-loggers, and openly readable audit log files.  

<table class="wiki_table"><tbody><tr><td>**Risk Number**  
</td><th>Arc  
</th><th>Risk  
</th><th>**Likelihood**  
 **(L, M, H)**  
</th><th>Impact  
 (L, M, H)  
</th><th>Mitigation  
</th><th>Notes  
</th></tr><tr><td>2  
</td><td>3.2 6.2 9.2  
</td><td>DNS can be spoofed to return an attacker’s IP addresses rather than the correct ones. This could cause messages to be sent to an attacker’s system.  
</td><td>L  
</td><td>L  
</td><td>
*   Messages can only be decrypted by the party holding the private key corresponding to the public certificate used for encryption. Therefore encrypted messages can travel in the wild without risk to the contents.
*   TLS can be used at the SMTP level. This would add another layer of authentication that must be passed, but also adds to complexity of configurations.
    *   TLS is only guaranteed to the first point. This is an important step, but there may be other SMTP

</td></tr><tr><td>3  
</td><td>7  
</td><td>potential exposure of TO/FROM routing information (network, wireless, internet mailstop). Exposing that the addressee identified in the TO is having a private conversation with the addressee identified by the FROM.  

*   provider-to-provider -- There is no knowledge of the topic of the conversation, it could be golf.
*   provider-to-patient -- There is knowledge of types of conversations where the provider is a specialist

</td><td>L  
</td><td>L  
</td><td>Each Recipient is in control of who they provide their endpoint address to, and each Sender is in control of who they communicate with.  

*   TLS can be used at the SMTP level. This would add another layer of authentication that must be passed, but also adds to complexity of configurations.
    *   TLS is only guaranteed to the first point. This is an important step, but there may be other SMTP

</td></tr><tr><td>5  
</td><td>1,7,12  
</td><td>Denial-of-Service attack against a recipient that no longer receives messages that have been routed away from their SMTP server.  
</td><td>L  
</td><td>L  
</td><td>
*   Because such an attack would impact all addresses on a given domain, a monitoring system could decide that the lack of incoming messages over a period of time warrants investigation.
*   SMTP and POP3, IMAP4, etc. servers openly accepting connections from the Internet should use standard firewall and intrusion detection systems to protect their interfaces.

</td></tr><tr><td>6  
</td><td>7  
</td><td>An attacker intercepts message traffic (network, wireless, internet mailstop).  
</td><td>L  
</td><td>L  
</td><td>The message content is encrypted and signed using S/MIME so no exposure of PHI.  
</td></tr><tr><td>7  
</td><td>12  
</td><td>Interception of POP/IMAP authentication credentials through unsecured network (e.g. wireless) sniffing. Exposed credentials allows attacker to spoof the account.  
</td><td>M  
</td><td>H  
</td><td>
*   TLS can be used at the POP/IMAP level, but adds complexity of configuration

</td></tr><tr><td>8  
</td><td>7  
</td><td>e-Mail is received that is not S/MIME protected  
</td><td>M  
</td><td>M  
</td><td>Security agent can reject all non-Secure (S/MIME) messages  
</td></tr><tr><td>9  
</td><td>3/4  
</td><td>DNS may be spoofed and an attacker’s certificates returned instead of the correct ones. If the attacker could then obtain a message encrypted with their certificates, they could read the contents.  
</td><td>L  
</td><td>H  
</td><td>The agent will verify that each certificate fetched traces its issuing chain to a locally-configured trusted anchor certificate. This mitigates the attack because it could only be performed by an already-trusted actor.  
</td></tr><tr><td>10  
</td><td>3/4  
</td><td>Denial-of-Service attack against a recipient that receives un-decryptable messages because of spoofed DNS.  
</td><td>L  
</td><td>L  
</td><td>Because invalid encryption certificates should be a rare occurrence, the agent can raise system events when they occur. Standard system monitoring systems can be used to detect when the number of such events warrants investigation.  
</td></tr><tr><td>11  
</td><td>3/4  
</td><td>Denial-of-Service attack against all recipients using certificates issued by an intermediate authority (between the endpoint certificate and configured anchor certificate).  
</td><td>L  
</td><td>L  
</td><td>These certificates will be flagged as invalid using the agent’s chain verification logic. Because this should be a rare occurrence, standard system monitoring system can be used as above to detect when the number of such events warrants investigation.  
</td></tr><tr><td>12  
</td><td>3/4  
</td><td>Denial-of-Service from a compromised CLR or OCSP service? Investigate further.  
</td><td>L  
</td><td>L  
</td></tr><tr><td>13  
</td><td>5  
</td><td>If the SMTP server is compromised, an internal or external attacker could steal queued messages.  
</td><td>L  
</td><td>H  
</td><td>Because the messages are encrypted using a key only available to the intended recipient, only TO/FROM routing information is vulnerable.  

 To protect the routing information, it is recommended that persistent message queues be further encrypted at rest using system-provided mechanisms. This cannot completely defeat an internal attack, but can greatly reduce the vulnerable surface.  
</td></tr><tr><td>14  
</td><td>11  
</td><td>If the SMTP server is compromised, an internal or external attacker could steal messages held in endpoint mailboxes.  
</td><td>L  
</td><td>H  
</td><td>The simplest mitigation for this attack is to use system-provided mechanisms to encrypt the at-rest message stores. This will not completely defeat an internal attacker, but will greatly reduce the vulnerable surface.  

 NOTE: One possibility here is to change the order of operations; if arcs 8-10 are performed AFTER arc 12 (e.g., messages are decrypted on fetch rather than receipt), message contents would remain encrypted at rest and mitigate this threat except for TO/FROM routing information.  
</td><td>Assessed as Low probability based on the fact that the service is known by the CE as a sensitive resource.  
</td></tr><tr><td>15  
</td><td>1  
</td><td>Improperly configured clients could send messages to an insecure system.  
</td><td>M  
</td><td>L  
</td><td>Because the messages are encrypted using a key held only by the intended receiver, no content will be disclosed.  
</td></tr><tr><td>16  
</td><td>1,12  
</td><td>Protocol-specific attacks. If SMTP, IMAP4, POP3 or proprietary servers or protocols are found to have flaws, the system could behave in unpredicatable ways.  
</td><td>M  
</td><td>H  
</td><td>This is a risk of any system; mitigation is to use best practices for software development, choose technology from trusted vendors or organizations, and apply patches and updates in a timely manner.  
</td></tr><tr><td>17  
</td><td>1,12  
</td><td>Man-in-the middle attacks. If an attacker is able to insert themselves between the client and server, PHI could be disclosed.  
</td><td>L  
</td><td>H  
</td><td>The channels shoudl be encrypted using a technology such as TLS or SSL that providers server-level identity assurance, or contained within a trusted environment such as a secure intranet.  
</td></tr><tr><td>18  
</td><td>1,12  
</td><td>Theft of client credentials . If an attacker obtains account credentials, they could appear as a valid user and view PHI.  
</td><td>M  
</td><td>H  
</td><td>Adopt safe credential-management policies as appropriate such as (but not limited to):  

*   Regular forced password changes
*   Use of passwords resistant to dictionary attack
*   Second factor authentication
*   Server-level restriction of incoming connections by IP or other parameter
*   Regular deactivation of inactive accounts

</td></tr><tr><td>19  
</td><td>1,12  
</td><td>Clear text messages saved on client machine (sent folder for arc 1, inbox and message folders for arc 12). If a machine is compromised or stolen, PHI could be disclosed.  
</td><td>M  
</td><td>H  
</td><td>Adopt safe asset-management policies as appropraite such as (but not limited to):  

*   Drive-level encryption
*   Limitations on physical location of machines
*   Regular cleanup of drives

The attack can also be mitigated by using web-based or remote-application (Citrix, etc.) access to clients rather than local applications.  
</td></tr><tr><td>20  
</td><td>1,12  
</td><td>DNS spoofing of server addresses. A client tricked into contacting an attacker's server could disclose credentials or PHI.  
</td><td>L  
</td><td>H  
</td><td>
*   Adopt mutual TLS or SSL for conversations with the server.
*   Limited the trusted CAs for the connections.
*   Run services in a trusted environment where safety of DNS resources is assumed.

</td></tr><tr><td>22  
</td><td>2  
 8  
</td><td>The trusted service provider has access to the private keys of the sender and receiver and thus can decrypt anything sent to these endpoints and can impersonate any of these endpoints  
</td><td>L  
</td><td>H  
</td><td>The Gateway is a known entity that is explicitly considered. The Gateway should be carefully managed and audited.  

 The XDR endpoints could all use the same Certificate and thus private key. Thus the encryption is targeted to the gateway and messages are signed by the gateway.  
</td></tr></tbody></table></div></div></div>
