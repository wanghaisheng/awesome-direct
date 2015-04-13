Direct Project Security Overview


Introduction:
The Direct Project is a set of specifications and service descriptions that, when implemented within a strong policy framework, enables simple, secure point-to-point electronic messages between health care participants. The Direct Project security specifications provide a set of functional requirements that need to be implemented by the various Direct Project technologies and protocols in order to satisfy the message handling policy recommendations of the HIT Policy Committee. In simple terms Direct Project security specifications are intended to say “messages go where they are meant to, are not altered during transmission, and are not seen by anyone for whom they are not intended."

In the same way that clinicians currently do not assume that it is safe to fax protected health information to anyone with a fax number, or mail PHI to anyone with a post office address, Direct Project users should not assume that it is safe to send messages to any Direct Project address. Direct Project users will need to establish real-world trust relationships with other Direct Project users on their own terms, but once they have established this real-world trust, they can be sure that a Direct Project network will securely deliver Direct Project messages to the trusted Direct Project user.

As much as possible, the design of the Direct Project secure network will not force Direct Project users to adopt trust policies they might otherwise have rejected, or reject trust policies they might otherwise have adopted. However, in order to build a secure platform for the transmission of PHI, Direct Project must specify acceptable protocols and technologies that Direct Project Implementers will use to communicate secure network PHI messages over multiple possibly insecure, temporary inter-network interfaces for Direct Project users. For example, an insecure and compromised network node along the transmission path should not allow an attacker to modify information en route and alter the contents of the message, pretending to each legitimate participant to be the beginning and endpoint of the message. (Man in the Middle attack).

In some cases, these protocols and technologies will come with specific configuration options that will have policy implications and may also present constraints that the Direct Project will force on the trust policies of its users.

The Direct Project security protocols and technologies are an established reference point for currently defined and accepted "levels of assurance" within the trust framework that protect participants against well known existing network security threats regarding confidentiality, message integrity and interception as defined by NIST.

The decision to use the ITU-T X.509v3 standards for public key infrastructure (PKI) based on IETF Internet certificate profiles (PKIX) will enable Direct Project users to create trust policies that enable network encryption by the use of digital certificates, public and private keys and certificate authorities. The content encapsulated in the messaging protocol is secured using the Secure/Multipurpose Internet Mail Extensions (S/MIME) protocols.



If two Direct Project users trust each other in the real world, and can mutually agree on standards and policies for handling PHI, they will be able to configure a Direct Project implementation to securely send messages containing PHI to each other.

Assumptions
The following is a set of assumptions that provide “context” for the Direct Project security specifications

    Policy guidance for the Direct Project will be provided by the NHIN Workgroup of the HIT Policy Committee and will not be decided within the Direct Project itself. Organizations must choose the policies and practices that support their specific environments.
    The Direct Project will be conformant with applicable federal and state laws, including but not limited to security and privacy laws.
    The Sender has made the determination that it is clinically and legally appropriate to send the information to the Receiveroutside of the Direct Project Specifications.
        The sender has fulfilled all legal, regulatory and policy requirements such as consent, authorization, accounting of disclosure, privacy notices, data use restrictions incumbent on the receiver, and the like prior to initiating transport.
        The receiver of the information agrees to use the information for the purpose it was sent, not for other purposes.
    The Sender has verified that the Reciever's address is correct. The Sender may obtain the Receiver's address via any number of appropriate means that are currently not specified by the Direct Project.
    The Sender knows that the patient’s privacy preferences are being honored, because the patient has consented to the sending of information to the Receiver for the specific purpose.
    The Sender and Receiver have ensured that their HISP, if one is used, is authorized to act as such and is authorized to handle PHI according to law and policy.
    The Sender and Receiver do not require common or pre-negotiated patient identifiers. Similar to the exchange of fax or paper documents, there is no expectation that a received message will be automatically matched to a patient or automatically filed in an EHR.


Goals of Direct Project Security specifications
The goals of the Direct Project security specifications are to achieve the following:

    Enabling Message Handling Trust between Participants:
        Verify the identity of a sender when information is received
        Allow a Direct Project participant to specify other participants they trust to exchange information
        Provide non-repudiation service that assures the origin of information
    Protecting the Information Exchanged:
        Ensure information including PHI that is exchanged between Direct Project participants is kept confidential during transit.
        Verify that information exchanged between Direct Project participants was not altered in transit.
    Policy
        Ensure that the technology choices enable different policy frameworks that might exist across various organizations.


Enabling Trust between Participants

In general terms an entity A (e.g., provider, hospital, patient) is said to trust another entity B only when entity A believes that entity B behaves correctly. In other words Trust is an indication of confidence in another to fulfill their promises and conform to some basic policies, ethical codes and adherence to the law.

In the context of the Direct Project Security specifications, the parameters of trust are constrained to the Goals and Assumptions noted above. That is to say, given that the sender intends to send to the receiver (that is, trusts the receiver with respect to the trust issues in the Assumptions), the Direct Project Security specifications must ensure that the transaction conforms to the listed Goals.

Options For Enabling Trust

The Direct Project considered the following models for establishing messaging handling trust:

    Requiring individual or organization by organization determinations of messaging handing trust
    Requiring a single national floor set of messaging handling trust policies and a single national certificate trust anchor
    Allowing multiple overlapping circles of trust that are expected to converge over time


The determination of the project was that the first option inherently does not scale, and the second option was not achievable in the timeframe of interest. We therefore chose a "Circles of Trust" model.

Background on Trust and X509 Certificates

This short primer of terms and concepts is important to understand the material that follows.

A Certificate, in the context of this document, is a standard X509 Certificate. The Certificate has certain properties that allow software to verify that the certificate was issued to the person or organization it purports to, that it is in current standing, etc. Certificates are generally signed by a second certificate held by a Certificate Authority, which establishes policies by which it will issue signed certificates. By inspecting certificates, it is possible to prove that the Certificate was issued by the trusted Certificate Authority, by inspecting a chain of certificates that root to a known Trust Anchor.

If you trust a Certificate Authority's issuance policy and you hold a Trust Anchor certificate that corresponds to the Certificate Authority, you can prove that any certificate that purports to be trustworthy does was, in fact, issued by the Certificate Authority (or by a secondary Authority that, in turn, is trusted by the root Certificate Authority). You can do so even if the certificate you hold was obtained through means you do not trust (e.g., by extracting it from a signature you do not yet trust, or over the possibly spoofed Domain Name System (DNS)).

Note that by "Certificate Authority" we do not mean the usual Certificate Authorities that issue TLS certificates used in ordinary browsers. The Certificate Authorities used for the purposes in this document are likely to either be special purpose for healthcare, or be highly trusted Certificate Authorities that are used for other similar purposes, such as issuing certificates for interoperability with the Federal Government. Note that the same organizations that maintain Certificate Authorities for electronic commerce may also maintain Certificate Authorities for these purposes, but the root Trust Anchors will almost certainly be different.

Circles of Trust

When users of the Direct Project make message handling trust decisions, they will often be able to do so in the context of trusting the identity assurance and authentication policies of an entire group of other Direct Project endpoints. A group of Direct Project endpoints who use certificates issued by a single Certificate Authority and agree to follow the identity assurance, authentication, security, and other policies of that Certificate Authority are considered a "Circle of Trust." By having large groups of endpoints who all agree to identical message handling policy positions, the number of trust decisions that Direct Project Users will need to make will be decreased. Hopefully, this will mean that the quality of each trust decision will be improved. The Circles of Trust concept is a critical part of the Direct Project innovation to enable distributed trust at scale, without sacrificing on the quality of trust decisions.

Each Circle of Trust is enabled by a single Certificate Authority, which signs the certificates of all of the endpoints of the Circle of Trust and publicly discloses what security posture is enforced within the Circle of Trust. The Direct Project will call such Certificate Authorities "Trust Anchors".

Decentralized Trust Model
Direct Project Implementations must allow configuration of one or more public certificates representing "anchors" that implement agreed-to message handing policies. Implementations must be able to allow distinct trust anchors for sending and receiving messages.

Because Direct Project implementations are required to be able to support more than one public certificate as a source of trusted signatures, the Direct Project has implicitly embraced a decentralized Trust Model. This model is compatible with the still forming **National Strategy for Trusted Identities in Cyberspace**. Under the decentralized model, Direct Project organizations will have the ability to choose which Certificate Authorities they will trust. A Direct Project implementation should not ship with a default list of trusted Certificate Authorities, which is a practice adopted by web browser implementers. Direct Project organizations should be allowed to make explicit trust decisions, by choosing which Certificate Authorities enforce security and privacy postures that are compatible with their local laws, cultures and values. In the Direct Project decentralized trust model approach, each Certificate Authority, using the appropriate linkage between certificates and policy documents, will create a Circle of Trust.

Some organizations may have constraints on which Certificate Authorities they may use. For example, Federal providers and agencies must conform to Federal Government polices regarding certificate authorities and certificate issuance. Nothing in this document or the Direct Project specifications require any organization to adopt a wider set of Circles of Trust than they are able to by policy.

Trust Anchors
All of the Direct Project endpoints that are signed by a single Certificate Authority agree to abide by the policies of that Certificate Authority. The Certificate Authority has the responsibility to perform the appropriate auditing to ensure that its policies are enforced. These Certificate Authorities are called "Trust Anchors." Some Certificate Authorities will choose to enforce very detailed and extensive security postures with frequent and thorough audits. Other Certificate Authorities may choose to enforce fewer security measures. The security posture and auditing levels that a given Certificate Authority enforces will be publicly published so that Direct Project users will be able to determine if it is a good decision to trust the Certificate Authority and all of its users. Direct Project users can configure their implementation with the “Trust Anchors” they are willing to trust for sending and receiving messages.

Direct Project endpoints might reasonably decide that they will only allow outgoing messages to other Direct Project endpoints who participate in Circles of Trust with comparable or higher levels of enforced security postures. However, they might also decide that they will accept incoming messages from Circles of Trust that have much lower standards of security and privacy standards. For instance, a clinic or hospital might choose to accept incoming messages from the users of a PHR system. The PHR system might not perform the same high level of identity assurance on its users that the hospital or clinic requires internally. While the hospital or clinic might be willing to receive messages from the PHR, they might choose to send outgoing messages only after they have manually verified that the address is actually owned by a particular patient.

The concept of “Trust Anchors” and the flexibility of configuring “Trust Anchors” distinctly for sending and receiving messages provides the required flexibility for Direct Project users to adopt various policies that they deem necessary for their organization.


Protecting Information Exchange
The Direct Project solution mandates the use of S/MIME as the means to provide end-to-end authenticity, confidentiality and integrity protection. S/MIME is a secure messaging infrastructure commonly available in e-mail systems today. This availability of the technology should drive fast adoption embracing good security practices for exchanging health information.

Message Authentication
The S/MIME mechanism can assure that a message received was received from a known Direct Project endpoint. It does this through the use of the digital certificates and trust relationship discussed above. Each message carries with it the certificate that was used by the sending Direct Project endpoint, and mechanisms in the S/MIME message will be used to authenticate the message as coming from that identity.
Message Encryption
The S/MIME protocol supports the capability to encrypt the message as targeted to only the destination Direct Project endpoint identity as described by that endpoint digital certificate. This encryption is mandatory in the Direct Project specifications to protect the confidentiality of the message.

Message Integrity
The S/MIME protocol supports the capability to sign the message as coming from the sender Direct Project endpoint identity as described by that endpoint's digital certificate. This signing capability is mandatory within the Direct Project specifications and allows receivers to be assured of the identity of the sender. The Message Signing and Message Encryption capabilities of S/MIME will provide the necessary message integrity of Direct Project Messages.

Transport Security
For transmission of already S/MIME signed and encrypted content the Direct Project solution is encouraged to support transport level security. This capability can be used to assure that connections are made only to trusted systems, that the network communications are additionally encrypted and integrity protected. For transmission of any content that has not already been encrypted (e.g., incoming SMTP or outgoing IMAP connections to the Full Service HISP), transport level security and encryption is mandated.

Use of Other Transport Protocols

While all Direct-compliant implementations must support S/MIME signed and encrypted documents over SMTP, implementations may offer support of other protocols when both sender and receiver mutually agree to do so. Such support may include, for instance, the Document Submission specification used in the Nationwide Health Information Network Exchange, based on IHE XDR. In such cases, the implementation must support methods to authenticate, encrypt and integrity check content to provide a high degree of trust. This often includes use of X509 Certificates using Transport Level Security (TLS) with mutual authentication (where both sides of the transaction ensure compliance with a single common known trust anchor prior to transmitting data).

Risk Assessments
Given the protections specified above, the Direct Project committee has executed Risk Assessments of some Deployment Architectures. These Risk Assessments include the identification of some residual risks that should be handled in the deployment or operational environment.

    The Sender must be sure they have the correct destination address and digital certificate to assure the content gets sent to the correct place.
    The Sender must be sure not to put sensitive information into the e-mail headers, as they are not protected.
    The Sender must be careful to always send sensitive content using the security of the Direct Project.
    If either Sender or Receiver utilize any external HISP, then care must be taken to assure that the HISP is protecting the content and processing it appropriately.


Suggested Readings for More Details:
More detail on the Security of the Direct Project can be found in the Content Security for Simple Health Transport

Authors and Editors:
Name
	Affiliation
Nagesh Bashyam
	Harris Corporation
Will Ross
	Redwood Mednet
John Moehrke
	GE
Fred Trotter
	Cautious Patient Foundation
Sean Nolan
	Microsoft
Brian Behlendorf
	ONC
Brett Peterson

Arien Malec
	ONC
John Moehrke
	GE Healthcare

Glossary

ITU-T United Nations organization that develops telecommunications protocols, Many IETF (Internet) protocols and standards are derived from ITU-T standards regarding security, such as X.509v3.

IETF: Internet Engineering Task Force Organization. The goal of IETF is to make the Internet work better by producing high quality, relevant technical documents that influence the way people design, use and manage the Internet.
Certificate Authority: In cryptography, a certificate authority or certification authority (CA) is an entity that issues digital certificates. The digital certificate certifies the ownership of a public key by the named subject of the certificate. In the model of trust relationships, a CA is a trusted third party that is trusted by both the subject (owner) of the certificate and the party relying upon the certificate. CAs are characteristic of many public key infrastructure (PKI) schemes.It is not required to have a CA to generate digital certificates that enable encryption between parties, however, a defined CA will normally support CA policy statements which specify risk levels, based on the amount of information which can be verified regarding an organization,or individual. These risk levels will be indemnified, and covered by an insurance policy if those policy goals are not met in the security implementation or compromise of a private key. The risk levels must be adjusted to the requirements of the participants in the message protocol, and are priced accordingly to how the certificates will be used, and the presence of other security and policy protocols that allow information to be corrected by authorized users.

Digital Certificate: In cryptography, a digital certificate, identity certificate, or attribute certificate, is an electronic document which uses a digital signature to bind together a public key with an identity which could be information such as the name of a person or an organization, their address, and so forth. The digital certificate cannot be used to verify that a public key belongs to an individual, unless it is bound to an entry in a directory which contains the certificate attributes, along with a look-up mechanism, (LDAP, X.500), or in the typical use case where it can be verified by a root certificate in a web browser's certificate store, where the certificate will be verified through a chain of certificate authorities. In a typical PKI scheme, the signature will be of a certificate authority (CA).Since identity attributes are generally immutable in the birth to death vital records cycle, the use of identity to proxy for authorization is likely to pose known risks, if an attribute certificate which enables access to a service can be used instead, since the attribute certificate will be valid for a fixed amount of time. By "timing out" identity digital certificates for S/MIME, users can typically be assured that their identity can be compromised for a set amount of time if they lose control over their private key, computer malfunction, or inability to revoke a compromised certificate. In the simplest (local) case, the user could inform the other party that the certificate was compromised directly.
Message Encryption: Message encryption is a process that encodes the data of a message so that unauthorized people cannot access it. The process of message encryption converts a message from readable text to scrambled or enciphered text, thus keeping the message content private. Only people who use a private key can decrypt and read such a message.
Message Signing: Message Signing is a scheme for demonstrating the authenticity of a digital message or document. Usage of valid digital signature gives a recipient reason to believe that the message was created by a known sender, and that it was not altered in transit. The sender typically signs the message with their private secret key and the receiver verifies the signature of the sender using their public key.
MIME: Mulltipurpose Internet Mail Extension. MIME is an Internet standard that extends the format of e-mail to support text in character sets other than ASCII, Non-text attachments, Message bodies with multiple parts, Header information in non-ASCII character sets. MIME's use, however, has grown beyond describing the content of e-mail to describing content type in general, including for many web applications.
PKI: Public Key Infrastructure. PKI is a set of hardware, software, people, policies, and procedures needed to create, manage, distribute, use, store, and revoke digital certificates.In cryptography, a PKI is an arrangement that binds public keys with respective user identities by means of a certificate authority (CA). The user identity must be unique within each CA domain. The binding is established through the registration and issuance process, which, depending on the level of assurance the binding has, may be carried out by software at a CA, or under human supervision.
S/MIME: Secure/Multipurpose Internet Mail Extensions is a standard for public key encryption and signing of MIME data.
X.509: X.509 is a standard that specifies, amongst other things, standard formats for public key certificates, certificate revocation lists, attribute certificates, and a certification path validation algorithm.
