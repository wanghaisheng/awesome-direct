This page is intended to model threats associated with a basic Direct Project conversation between two e-Mail clients, each using a "full service" e-Mail Client that provides e-Mail security (S/MIME), Address Book (key store), and DNS services. This configuration presumes no new development is necessary as all components of the solution are off-the-shelf today. Although messages may contain multiple recipients, we have constrained this model to only one for simplicity. The Source and Destination should not be considered mandated to both follow the same configuration, they are modeled together for simplicity of documentation. There are of course a number of other possible configurations that alter the threat profile; these will be evaluated on separate pages as appropriate.  

 This threat assessment followed the [Threat Model Process](/Threat+Model+Process). Other Deployment Models have been similarly assessed and are cataloged on the [Threat Models](/Threat+Models) page.  

# Pre-Conditions:

 All normal Direct [Consensus Proposal - Assumptions](/Consensus+Proposal) are in play. Specific to this configuration:  

1.  The Human/Organization that is sending the documents is responsible for verifying the address and obtaining the destination security keys. This is often done with a simple 'test' message that contains no sensitive data, but is confirmed as being correct through a direct phone call.

# Benefits:

*   No specialization (Direct) is needed. This configuration utilizes 100% off-the-shelf e-mail clients available today ([wikipedia comparison of e-mail clients](http://en.wikipedia.org/wiki/Comparison_of_email_clients)shows that there is a subset of e-mail clients that support S/MIME but it is still a significant (~20) number).
    *   Does not mandate TLS, but does offer it as a point-to-point additional protection (also commonly available today).
*   No PHI is exposed to any HISP
*   e-Mail client 'sent' mail folder is archive of items Disclosed. Inspection of this folder can determine if an inappropriate exposure (non-secured PHI) has happened Note that hard drive folders can also be used to store any e-Mail attachments for tracking and access.

# Drawbacks:

*   This configuration requires that the Sender or Receiver maintain an e-mail client that has the capability to handle S/MIME, and the address book.
*   This configuration is harder to automate inside an EHR/PHR workflow
    *   Use of Windows-MAPI interface can mitigate this problem
    *   Use of e-Mail toolkits that support the necessary functionality can mitigate this problem

# Diagram:

 The following diagram depicts the key components and arcs involved in this conversation:  
 <div style="text-align: center">![NHIN-Direct-Simple-SMTP.jpg](/file/view/NHIN-Direct-Simple-SMTP.jpg/161456165/576x432/NHIN-Direct-Simple-SMTP.jpg "NHIN-Direct-Simple-SMTP.jpg")</div>
## Arc 1 - Source e-Mail Client Looks up Keys based on Destination Direct Address

 In this arc, the e-mail client uses the native address book of the e-mail client to retrieve the certificate of the destination address. The certificate is validated to assure it is whole, complete, has not expired and is not revoked. Because this arc occurs completely within the trusted boundaries of the Source, it is expected that the agent will be able to securely make this request using standard secure programming practices. We will not further model this arc in this document.  

## Arc 2 - Source e-Mail Client fetches locally-configured private keys and anchor certificates

 In this arc, the e-Mail client queries a local configuration resource such as a database, file system or certificate store for the private keys and anchor certificates associated with the source address. Because this arc occurs completely within the trusted boundaries of the HISP, it is expected that the agent will be able to securely make this request using standard secure programming practices. We will not further model this arc in this document.  

## Arc 3 - Source e-Mail Client resolves Destination SMTP Server addresses

 In Arc 3, the e-Mail Client queries its locally-configured DNS server for the MX records corresponding to the domain name of the destination address. The public DNS hierarchy is utilized, that is the configured DNS server forwards non-local MX requests through the distributed public DNS hierarchy.  

## Arc 4 - Source e-Mail Client sends encrypted/signed message to Destination SMTP Server

 In this arc, the source e-Mail Client establishes an normal SMTP connection (non-secured) to the destination SMTP server. The encrypted message is sent over this channel using standard SMTP mechanisms.  

## Arc 5 - Destination e-Mail Client fetches message from Destination SMTP Server

 In this arc, the destination client first establishes an normal (non-secured) POP3 or IMAP4 connection to its configured mail server. The client then sends standard requests to receive message headers and contents over this connection.  

## Arc 6 - Destination e-Mail client fetches locally-configured private keys and anchor certificates

 In this arc, the e-Mail client queries a local configuration resource such as a database, file system or certificate store for the private keys and anchor certificates associated with the source address. Because this arc occurs completely within the trusted boundaries of the Destination, it is expected that the agent will be able to securely make this request using standard secure programming practices. We will not further model this arc in this document.  

## Arc 7 - Destination e-Mail client fetches and verifies public certificates for source address

 Signature validation is done using the certificates embedded in the message itself, so these certificates do not need to be fetched. However, as the e-Mail client explores the certificates and issuing chain hierarchy searching for a configured anchor certificate, intermediate certificates may need to be fetched and in all cases revocation status must be determined.  

# Risk Assessment and Mitigation Plan

 In the interests of keeping scope on the Risk Assessment: We assume good programming practices and good business operational practices. For example we do not include risks of poor protocol implementation that would expose buffer-overflows, or poor operational environment that allows sharing of accounts, key-loggers, and openly readable audit log files.  

<table class="wiki_table"><tbody><tr><td>**Risk Nunber**  
</td><th>Arc  
</th><th>Risk  
</th><th>**Likelihood**  
 **(L, M, H)**  
</th><th>Impact  
 (L, M, H)  
</th><th>Mitigation  
</th><th>Notes  
</th></tr><tr><td>1  
</td><td>1  
</td><td>The user may send PHI and 'forget' to use S/MIME.  
</td><td>H  
</td><td>H  
</td><td>Although the pre-conditions (Assumptions) sets this as a responsibility of the sender, it is exposed here explicitly as a reasonable accidental risk.  

*   Some e-Mail clients can be configured to only send using S/MIME and will thus refuse to send to an address that can't be secured
*   Use of Integrated EHR/PHR with the e-mail infrastructure means user does not have access to e-Mail User Interface
*   e-Mail client 'sent' mail folder is archive of items Disclosed. Inspection of this folder can determine if an inappropriate exposure (non-secured PHI) has happened
*   Use of "Data Loss Prevention" systems to detect and block sensitive information from leaving an organization (benefit of using non-secured channel is that it can be inspected) (see: [Gartner report](http://www.gartner.com/DisplayDocument?id=1379314))
*   Use of cloud based secure-email where the necessary functionality (see above) is more easily managed.

</td></tr><tr><td>2  
</td><td>3  
</td><td>DNS can be spoofed to return an attacker’s IP addresses rather than the correct ones. This could cause messages to be sent to an attacker’s system.  
</td><td>L  
</td><td>L  
</td><td>
*   Messages can only be decrypted by the party holding the private key corresponding to the public certificate used for encryption. Therefore encrypted messages can travel in the wild without risk to the contents.
*   TLS can be used at the SMTP level. This would add another layer of authentication that must be passed, but also adds to complexity of configurations.
    *   TLS is only guaranteed to the first point. This is an important step, but there may be other SMTP

</td></tr><tr><td>3  
</td><td>4/5  
</td><td>potential exposure of TO/FROM routing information (network, wireless, internet mailstop). Exposing that the addressee identified in the TO is having a private conversation with the addressee identified by the FROM.  

*   provider-to-provider -- There is no knowledge of the topic of the conversation, it could be golf.
*   provider-to-patient -- There is knowledge of types of conversations where the provider is a specialist

</td><td>L  
</td><td>L  
</td><td>Each Recipient is in control of who they provide their endpoint address to, and each Sender is in control of who they communicate with.  

*   TLS can be used at the SMTP level. This would add another layer of authentication that must be passed, but also adds to complexity of configurations.
    *   TLS is only guaranteed to the first point. This is an important step, but there may be other SMTP

</td></tr><tr><td>4  
</td><td>4/5  
</td><td>When User uses e-mail client - potential for user to place PHI into the subject field, even though we caution against it. The SUBJECT is not commonly protected by current off-the-shelf email systems. exposure of SUBJECT information (network, wireless, internet mail stop). Exposing what ever information is placed into the subject line.  

*   Note when an automated process like EHR/PHR is using the Direct specification there is no risk.

</td><td>M  
</td><td>L  
</td><td>Could MANDATE specific string be in the SUBJECT field - For Example: "Direct"  

 Could be mitigated with TLS  

*   TLS is only guaranteed to the first point. This is an important step, but there may be other SMTP

 Could use more advanced form of S/MIME that is not commonly implemented. (Discourage as a mitigation)  

 Suggest that this risk be documented and flow to training  
</td></tr><tr><td>5  
</td><td>4  
</td><td>Denial-of-Service attack against a recipient that no longer receives messages that have been routed away from their SMTP server.  
</td><td>L  
</td><td>L  
</td><td>
*   Because such an attack would impact all addresses on a given domain, a monitoring system could decide that the lack of incoming messages over a period of time, as well as failure of Source to receive of Delivery Receipts from Destination for a period of time,  
     warrants investigation.
*   SMTP servers openly accepting connections from the Internet should use standard firewall and intrusion detection systems to protect their interfaces.

</td></tr><tr><td>6  
</td><td>4/5  
</td><td>An attacker intercepts message traffic (network, wireless, internet mailstop).  
</td><td>L  
</td><td>L  
</td><td>The message content is encrypted and signed using S/MIME so no exposure of PHI.  
</td></tr><tr><td>7  
</td><td>5  
</td><td>Interception of POP/IMAP authentication credentials through unsecured network (e.g. wireless) sniffing. Exposed credentials allows attacker to spoof the account.  
</td><td>L  
</td><td>L  
</td><td>The message content is encrypted and signed using S/MIME so no exposure of PHI.  

*   TLS can be used at the POP/IMAP level, but adds complexity of configuration

</td></tr><tr><td>8  
</td><td>5  
</td><td>e-Mail is received that is not S/MIME protected  
</td><td>M  
</td><td>M  
</td><td>
*   the judgment of the receiving user can determine if the information should be trusted or not.
*   Many will choose to simply discard all non-secured as potentially SPAM.
*   The Destination is not responsible for assessing exposure as that is a sender responsibility, although a notification (return e-mail) to the sender indicating that a non-secured e-mail was received (without details) would be recommended as professional courtesy. Care must be taken to prevent automated responses to responses.

</td></tr></tbody></table></div><div id="pageEditor" class="editorWrapper editorLayer"><form action="/page/edit/Threat+Model+-+Simple+SMTP" method="post" class="editorForm" id="editorForm"><input name="wikispacesFormToken" value="96dfff23be9a1554b2a7b490cd1e14db0f73db37" type="hidden"><div class="jovise"><input class="form-control" name="wikispaces_user_id" value="user-1428917542" type="text"></div><div class="jovise"><input class="form-control" name="goto" autocomplete="off" type="text"></div><div class="jovise"><input class="form-control" name="mode" autocomplete="off" type="text"></div><div class="movkp"><input class="form-control" name="version" autocomplete="off" type="text"></div><div class="movkp"><input class="form-control" name="pageComments" autocomplete="off" type="text"></div><div class="mimw"><input class="form-control" name="saveAndContinue" autocomplete="off" type="text"></div><div class="wsh"><input class="form-control" name="comment" autocomplete="off" type="text"></div><div class="jowic"><input class="form-control" name="commentsDisabled" autocomplete="off" type="text"></div><div class="movkp"><input class="form-control" name="contentPresent" autocomplete="off" type="text"></div><div class="wsh"><input class="form-control" name="editorCleanup" autocomplete="off" type="text"></div><div class="jovise"><input class="form-control" name="WikispacesEditorContent" autocomplete="off" type="text"></div><input name="wikispacesASFormToken" value="7f3b3ebdcf4fc9a47567c0922f6c6cf5783414d3" type="hidden"> <input title="goto" name="3306c4ecb7081c6c4ad7cfa39e85f0d05d82c604" value="" type="hidden"> <input title="mode" name="b9fe4af7e38089799853578b013d5eb27cd69741" autocomplete="off" value="visual" type="hidden"> <input title="version" name="78d2be034fa6c6f2f39b5a5e3db7d138727f9ea9" autocomplete="off" value="390814434" type="hidden"> <input title="pageComments" name="79a931652de609ac4d7b6c21da226061eda67c1b" autocomplete="off" value="" type="hidden"> <input title="saveAndContinue" name="f2c14e2b39ca8162aca530e89cc33dcd0db74c6c" value="0" type="hidden"> <input title="comment" name="d133e8920fcc749e0f783ca6634d5d7a4802b9f5" value="" type="hidden"> <input title="commentsDisabled" name="129038e27929cef548d3ec76344ed340e0ed7391" autocomplete="off" value="" type="hidden"> <input title="contentPresent" name="9443416c266793a6c20bdfd946ee9fb8ce237a49" autocomplete="off" value="" type="hidden"> <input title="editorCleanup" name="c8215cc82c266958e1fd3fb07169d94a8429f0ca" autocomplete="off" value="" type="hidden"> <noscript><h2>Javascript Required</h2> <p>You need to enable Javascript in your browser to edit pages.</p> </noscript></form><div id="plainTextEditor" style="position:relative;display:none" class="editorLayer">
[help on how to format text](http://help.wikispaces.com/help.wikitext)

</div></div></div></div>
