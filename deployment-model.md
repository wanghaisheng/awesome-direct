[Documentation and Testing Workgroup](/Documentation+and+Testing+Workgroup) > [Documentation Priorities](/Documentation+Priorities) > Deployment Models</span>  

### Status: This document achieved IG [Consensus](/Deployment+Models+Overview+-+Call+for+Consensus) on 12/23/2010.

 Author: John Moehrke  
 State: consensus  

# Introduction

 This article describes the various models of deployment for implementation of Direct Project point-to-point messaging. It details the various architectural components and their responsibilities for performing the necessary tasks for Direct Project messaging, depending on the model implemented. This will help describe [the Direct Project specification](/Simple+Health+Transport) and define the tests for conformance.  
 <div id="toc">
# Table of Contents

<div style="margin-left: 3em;">[Status: This document achieved IG Consensus on 12/23/2010.](#x--Status: This document achieved IG Consensus on 12/23/2010.)</div><div style="margin-left: 1em;">[Introduction](#Introduction)</div><div style="margin-left: 2em;">[Mixing and Matching](#Introduction-Mixing and Matching)</div><div style="margin-left: 2em;">[Tasks](#Introduction-Tasks)</div><div style="margin-left: 3em;">[Sending](#Introduction-Tasks-Sending)</div><div style="margin-left: 3em;">[Receiving](#Introduction-Tasks-Receiving)</div><div style="margin-left: 1em;">[Deployment Models](#Deployment Models)</div><div style="margin-left: 2em;">[A. E-mail client with Full Service HISP](#Deployment Models-A. E-mail client with Full Service HISP)</div><div style="margin-left: 3em;">[Preconditions](#Deployment Models-A. E-mail client with Full Service HISP-Preconditions)</div><div style="margin-left: 3em;">[Benefits](#Deployment Models-A. E-mail client with Full Service HISP-Benefits)</div><div style="margin-left: 3em;">[Drawbacks](#Deployment Models-A. E-mail client with Full Service HISP-Drawbacks)</div><div style="margin-left: 3em;">[E-mail Client Configuration](#Deployment Models-A. E-mail client with Full Service HISP-E-mail Client Configuration)</div><div style="margin-left: 3em;">[Risk Assessment](#Deployment Models-A. E-mail client with Full Service HISP-Risk Assessment)</div><div style="margin-left: 2em;">[B. E-mail Client Using Native S/MIME](#Deployment Models-B. E-mail Client Using Native S/MIME)</div><div style="margin-left: 3em;">[Preconditions](#Deployment Models-B. E-mail Client Using Native S/MIME-Preconditions)</div><div style="margin-left: 3em;">[Benefits](#Deployment Models-B. E-mail Client Using Native S/MIME-Benefits)</div><div style="margin-left: 3em;">[Drawbacks](#Deployment Models-B. E-mail Client Using Native S/MIME-Drawbacks)</div><div style="margin-left: 3em;">[Risk Assessment](#Deployment Models-B. E-mail Client Using Native S/MIME-Risk Assessment)</div><div style="margin-left: 2em;">[C. Web Portal or RESTful API](#Deployment Models-C. Web Portal or RESTful API)</div><div style="margin-left: 3em;">[Preconditions](#Deployment Models-C. Web Portal or RESTful API-Preconditions)</div><div style="margin-left: 3em;">[Benefits](#Deployment Models-C. Web Portal or RESTful API-Benefits)</div><div style="margin-left: 3em;">[Drawbacks](#Deployment Models-C. Web Portal or RESTful API-Drawbacks)</div><div style="margin-left: 3em;">[Risk Assessment](#Deployment Models-C. Web Portal or RESTful API-Risk Assessment)</div><div style="margin-left: 2em;">[D. EHR or PHR with integrated S/MIME functionality](#Deployment Models-D. EHR or PHR with integrated S/MIME functionality)</div><div style="margin-left: 3em;">[Preconditions](#Deployment Models-D. EHR or PHR with integrated S/MIME functionality-Preconditions)</div><div style="margin-left: 3em;">[Benefits](#Deployment Models-D. EHR or PHR with integrated S/MIME functionality-Benefits)</div><div style="margin-left: 3em;">[Drawbacks](#Deployment Models-D. EHR or PHR with integrated S/MIME functionality-Drawbacks)</div><div style="margin-left: 3em;">[Risk Assessment](#Deployment Models-D. EHR or PHR with integrated S/MIME functionality-Risk Assessment)</div><div style="margin-left: 2em;">[E. Direct Project to/from XDR (e.g., NHIN Exchange)](#Deployment Models-E. Direct Project to/from XDR (e.g., NHIN Exchange))</div><div style="margin-left: 2em;">[E.1 Direct Project sending to XDR with Trusted Service Provider (e.g. NHIN Exchange)](#Deployment Models-E.1 Direct Project sending to XDR with Trusted Service Provider (e.g. NHIN Exchange))</div><div style="margin-left: 3em;">[Preconditions](#Deployment Models-E.1 Direct Project sending to XDR with Trusted Service Provider (e.g. NHIN Exchange)-Preconditions)</div><div style="margin-left: 3em;">[Benefits](#Deployment Models-E.1 Direct Project sending to XDR with Trusted Service Provider (e.g. NHIN Exchange)-Benefits)</div><div style="margin-left: 3em;">[Drawbacks](#Deployment Models-E.1 Direct Project sending to XDR with Trusted Service Provider (e.g. NHIN Exchange)-Drawbacks)</div><div style="margin-left: 3em;">[Risk Assessment](#Deployment Models-E.1 Direct Project sending to XDR with Trusted Service Provider (e.g. NHIN Exchange)-Risk Assessment)</div><div style="margin-left: 2em;">[E.2 Direct Project receiving from XDR sender using a Trusted Service Provider (e.g. NHIN Exchange)](#Deployment Models-E.2 Direct Project receiving from XDR sender using a Trusted Service Provider (e.g. NHIN Exchange))</div><div style="margin-left: 3em;">[Preconditions](#Deployment Models-E.2 Direct Project receiving from XDR sender using a Trusted Service Provider (e.g. NHIN Exchange)-Preconditions)</div><div style="margin-left: 3em;">[Benefits](#Deployment Models-E.2 Direct Project receiving from XDR sender using a Trusted Service Provider (e.g. NHIN Exchange)-Benefits)</div><div style="margin-left: 3em;">[Drawbacks](#Deployment Models-E.2 Direct Project receiving from XDR sender using a Trusted Service Provider (e.g. NHIN Exchange)-Drawbacks)</div><div style="margin-left: 3em;">[Risk Assessment](#Deployment Models-E.2 Direct Project receiving from XDR sender using a Trusted Service Provider (e.g. NHIN Exchange)-Risk Assessment)</div><div style="margin-left: 1em;">[References](#References)</div></div>A. E-mail client that does not use S/MIME  

*   This deployment model uses a common e-mail functionality with a HISP service that intercepts all outbound/inbound e-mail messages. This HISP service is fully trusted and will apply the security functionality to encrypt and sign outbound e-mail; and decrypt and validate signatures on inbound e-mail. The prime advantage of this deployment model is to move the complexity of security to an external service.

B. E-mail client using native S/MIME  

*   This deployment model requires an e-mail system that supports the Security requirements. This e-mail system could be a fully functional e-mail client, or a common enterprise class e-mail system. The prime advantage of this deployment model is that the security is truly end-to-end which means that there is no third-party that has access to the PHI.

C. Web portal or RESTful API  

*   This deployment model uses an external service to provide all of the requirements of the Direct Project. In this model the user interacts using only their Internet Browser or where the client is an application (e.g., EHR or PHR) using an application programming interface (e.g., RESTful API or SOAP API, etc...) in a way that is defined by the service they have. This Browser/API interaction is not further specified by the Direct Project, but expect that there will be creative user interfaces. The external service provider is fully trusted and responsible for content packaging, addressing, security, and delivery. The prime advantage of this deployment model is that the end-user needs only a browser.

D. EHR or PHR with integrated S/MIME functionality  

*   This deployment model integrates all of the requirements into the EHR or PHR application. In this model the user is likely to be given a very simplified interface simply to select the documents to send and the destination address. The user interface is not defined by the Direct Project and would be dependent on the EHR or PHR supplier. The prime advantage of this is transparency to the user.

E. Direct Project to/from XDR (e.g., Nationwide Health Information Network Exchange)  

*   There are Health Information Exchanges that are using XDR for push workflows. These health information exchanges would like to send to and receive from participants that are using [the Direct Project specification](/Simple+Health+Transport). This deployment model shows how this can happen in an integrated way. The prime advantage of integrating these two specifications results in two different protocols being able to be used without changes to either endpoint.

 Certificate Management deployment models are described in the [Direct Project Security Overview](/Direct+Project+Security+Overview) including certificate issuance, validation, discovery, and trust models.  

## Mixing and Matching

 The deployment models shown are provided as a guide and are neither normative nor exhaustive. The reader should no![NHIN-Direct-Spec-Picture.PNG](/file/view/NHIN-Direct-Spec-Picture.PNG/162892573/NHIN-Direct-Spec-Picture.PNG "NHIN-Direct-Spec-Picture.PNG")te that **any deployment model on the sending side can communicate with any deployment model on the receiving side**, because of [the Direct Project specification](/Simple+Health+Transport) are common to all deployment models. This commonality allows the sender and receiver to choose very different deployment models. [The Direct Project specification](/Simple+Health+Transport) is shown in the diagrams using the graphic to the right "SMTP+S/MIME." This graphic should be seen as the standard that allows any one deployment model to communicate in a plug-and-play way to any other deployment model. Thus the deployment model that the sender uses is totally independent of the deployment model that the receiver uses.  

 Any participant using [the Direct Project specification](/Simple+Health+Transport) will likely be using the same deployment model for sending and receiving. It is possible, and totally allowed, that they could be using different deployment models them-self. For example the healthcare provider might use their EHR to send documents as the EHR might be very natural place to package up the documents of interest and with very simple user interface send the documents using [the Direct Project specification](/Simple+Health+Transport). This same healthcare provider might use an e-mail client to receive documents that are pushed using [the Direct Project specification](/Simple+Health+Transport) so that they can use the filtering and sorting functionality inherit in common e-mail clients today. This is not required of any healthcare provider, or software; but is given as an example of how important it is to see the deployment models as being independent. It is important to recognize that any deployment model can be used to send and any deployment model can be used to receive. The deployment model of the sender is independent of the deployment model of the receiver. The adherence to [the Direct Project specification](/Simple+Health+Transport) assures that any deployment model can communicate to any other deployment model, and in fact the receiver won't be able to tell what deployment model the sender is using.  

## Tasks

 [The Direct Project specification](/Simple+Health+Transport) imposes requirements on each of the parties in a Directed Exchange to ensure security and delivery of the message to the correct destination. The participants are typically the individual healthcare provider, healthcare organization, patient, or government reporting agent. The requirements are dependent on the role that the participant is taking for that specific transaction. Sometimes a participant will be sending documents, sometimes they will be receiving documents. These requirements can be broken down into tasks that software modules may provide, these tasks are also just as likely to be wholly integrated into a larger software component. We show these tasks broken out because they are the critical steps to sending or receiving; and the deployment models are showing how these tasks can be distributed between software inside an organization and outside in a internet accessible service.  

 The use of the word document is used here very broadly, requiring only that there be a MIME type to describe it in the MIME or XDM content packaging. This use of document is inclusive of structured and coded documents (e.g. CDA, CCD), unstructured documents (e.g. TXT, PDF), proprietary format (e.g. DOC), or encapsulated messages (e.g. HL7 v2 message).  

 In general, a Direct Project implementation must accomplish the following tasks. There may be additional tasks depending on the deployment architecture.  

### Sending

1.  Packaging content or implementing XDM
2.  Locating the destination's Address
3.  Locating Destination Certificate
4.  S/MIME-signing the content using Source Private Key
5.  S/MIME-encrypting the content using Destination Public Key
6.  Sending the S/MIME message via SMTP

### Receiving

1.  Receiving S/MIME message via SMTP
2.  Optional storage of the S/MIME (encrypted) message
3.  S/MIME decryption using Destination Private Key
4.  S/MIME signature verification using Source Public Key
5.  Optional POP/IMAP
6.  Optional storage of decrypted content

# Deployment Models

## A. E-mail client with Full Service HISP

 This model relies on a Full Service HISP.![NHIN-Direct-Deployment-Models-A-r2.png](/file/view/NHIN-Direct-Deployment-Models-A-r2.png/186450613/640x480/NHIN-Direct-Deployment-Models-A-r2.png "NHIN-Direct-Deployment-Models-A-r2.png")  

### **Preconditions**

*   All Direct Project [Consensus Proposal - Assumptions](/Consensus+Proposal) are in play.
*   The sender and receiver have ensured that agents of the sender and receiver (for example, HIO, HISP, intermediary) are authorized to act as such and are authorized to handle protected health information according to law and policy.

### **Benefits**

*   Since the e-mail client does not need to perform S/MIME functions, any e-mail client can be used.
*   The management of certificates and private keys is offloaded to the HISP, minimizing the burden on the client's implementation.

### **Drawbacks**

*   To assure confidentiality of information transmitted between Client and HISP, TLS is mandated for this deployment model
*   HISP has full access to PHI
*   Because the HISP has full access to the source's private key, it can impersonate the source

### **E-mail Client Configuration**

 The e-mail client will be configured to connect to the e-mail server hosted by the Full-Service-HISP in a manner that is secure. All other security is handled by the HISP. To configure the e-mail client:  

*   Obtain the SMTP and POP3 or IMAP domains for your HISP. Configure your client to use these domains for sending and receiving e-mail.
*   Configure these domains to use SSL, for both incoming and outgoing messages. This will secure the information transmitted between your e-mail client and the HISP. This step is required to protect personal health information passed in your message from the point it leaves your computer to the point it reaches the HISP's e-mail servers.
*   If your HISP instructs you to do so, configure the internal and external port numbers to the value supplied by your HISP.

### **Risk Assessment**

 This Deployment Model is representative of the use of the Direct Project Security Agent. This Deployment Model has been assessed for Risks in the [Threat Model - SMTP with Full Service HISPs.](/Threat+Model+-+SMTP+with+Full+Service+HISPs)  

## B. E-mail Client Using Native S/MIME

### Preconditions![NHIN-Direct-Deployment-Models-B-r2.png](/file/view/NHIN-Direct-Deployment-Models-B-r2.png/186451207/640x480/NHIN-Direct-Deployment-Models-B-r2.png "NHIN-Direct-Deployment-Models-B-r2.png")

*   All Direct Project [Consensus Proposal - Assumptions](/Consensus+Proposal) are in play. Specific to this configuration:
*   The human or organization that is sending the messages is responsible for verifying the address of the recipient and obtaining the destination's security keys. This is often done with a simple test message that contains no sensitive data but whose receipt is confirmed through, for example, a direct phone call.

### Benefits

*   No Direct Project specialization for the e-mail client is needed. A significant number of [off-the-shelf e-mail clients](http://en.wikipedia.org/wiki/Comparison_of_email_clients) support S/MIME today.
*   TLS is not mandated between Client and HISP, although it could be used to provide additional protection.
*   The HISP cannot access any PHI.

### Drawbacks

*   The Sender or Receiver must maintain an e-mail client that has the capability to handle S/MIME. The Sender or Reciever must also manage the address book.

### Risk Assessment

 This Deployment Model is representative of using a fully featured e-mail client. This Deployment Model has been assessed for risks in the [Threat Model - Simple SMTP](/Threat+Model+-+Simple+SMTP).  

## C. Web Portal or RESTful API

 This Deployment Model is representative a thin client using a Browser for user-interface, or where the client is an application (e.g., EHR or PHR) using an application programming interface (e.g., RESTful API, or SOAP API, etc). In all cases the Interface between the Client and its HISP is not defined by "The Direct Project".  
 ![NHIN-Direct-Deployment-Models-C-r2.png](/file/view/NHIN-Direct-Deployment-Models-C-r2.png/186452633/640x480/NHIN-Direct-Deployment-Models-C-r2.png "NHIN-Direct-Deployment-Models-C-r2.png")  

### Preconditions

*   All Direct Project [Consensus Proposal - Assumptions](/Consensus+Proposal) are in play.
*   The sender and receiver have ensured that agents of the sender and receiver (for example, HIO, HISP, intermediary) are authorized to act as such and are authorized to handle protected health information according to law and policy.

### Benefits

*   There is no client software needed beyond an Internet Browser.
*   The HISP can provide value added services such as address management, content packaging, etc.
*   The management of certificates and private keys is offloaded to the HISP, minimizing the burden on the client's implementation.

### Drawbacks

*   To assure confidentiality of information transmitted between Client and HISP, TLS is mandated for this deployment model
*   There needs to be some user authentication between the Client and their HISP
*   HISP has full access to PHI
*   Because the HISP has full access to the source's private key, it can impersonate the source

### Risk Assessment

 This Deployment Model is has not been assessed for risks, but would most closely represent [the ](/Threat+Model+-+SMTP+with+Full+Service+HISPs)[Threat Model - SMTP with Full Service HISPs.](/Threat+Model+-+SMTP+with+Full+Service+HISPs)  

## D. EHR or PHR with integrated S/MIME functionality

### Preconditions![NHIN-Direct-Deployment-Models-D-r2.png](/file/view/NHIN-Direct-Deployment-Models-D-r2.png/186452875/640x480/NHIN-Direct-Deployment-Models-D-r2.png "NHIN-Direct-Deployment-Models-D-r2.png")

 All Direct Project [Consensus Proposal - Assumptions](/Consensus+Proposal) are in play. Specific to this configuration:  

### Benefits

*   The Security is built directly into the EHR or PHR that is and will manage the content over the long term.
*   TLS is not mandated between Client and HISP, although it could be used to provide additional protection.
*   The HISP cannot access any PHI.
*   The EHR or PHR access controls can be leveraged
*   The EHR or PHR audit log can be leveraged

### Drawbacks

*   The EHR or PHR must integrate the Direct Project functionality into the native code, although Direct Project open source reference implementation.components may help developers to do this.
*   The EHR or PHR must manage addresses and digital certificates or integrate with a directory that does

### Risk Assessment

 This Deployment Model is has not been assessed for risks, but would most closely represents the [Threat Model - Simple SMTP](/Threat+Model+-+Simple+SMTP).  

## E. Direct Project to/from XDR (e.g., NHIN Exchange)

 For this deployment model, all prior deployment models for the Direct Project specification shown above are shown as a cloud, and the XDR environment (e.g., NHIN Exchange) as another cloud showing only the steps needed to bridge between them. With this deployment model there is the need to treat the XDR environment as a fully trusted party. While the XDR environment may be an NHIN Exchange participant, it could also be another entity that is not in NHIN Exchange, such as an HIO (local, regional, state) or an EHR that communicates via XDR.  

 This deployment model requires the conversion XDR to and from The Direct Project specification. There is special handling of this conversion of transport, content packaging, and metadata; please see the[ XDR and XDM for Direct Messaging](/XDR+and+XDM+for+Direct+Messaging+Landing+Page)specification for more details.  

 A system in an XDR environment can always choose to also be a Direct Project participant to get end-to-end security. The choice should be based on the Sender and Receiver relationship and their risk management strategy. A solution supporting both XDR and The Direct Project specification would be an application that can decide which transport to use to get to the desired destination.  

## E.1 Direct Project sending to XDR with Trusted Service Provider (e.g. NHIN Exchange)

### Preconditions![NHIN-Direct-Deployment-Models-E1-r2.png](/file/view/NHIN-Direct-Deployment-Models-E1-r2.png/173924303/640x480/NHIN-Direct-Deployment-Models-E1-r2.png "NHIN-Direct-Deployment-Models-E1-r2.png")

*   All Direct Project [Consensus Proposal - Assumptions](/Consensus+Proposal) are in play.
*   The sender and receiver have ensured that agents of the sender and receiver (for example, HIO, HISP, intermediary) are authorized to act as such and are authorized to handle protected health information according to law and policy.
*   Specifically the ultimate Receiver (E.1.7) has supplied the Sender with an address and digital certificate that route the message to the "Direct Project to XDR" Destination HISP. Thus the Receiver is explicitly indicating their trust of this Gateway.
*   The "Direct Project to XDR" Destination HISP is considered a trusted service provider. It advertises via DNS "MX" records that it is the endpoint for all of the addresses that it can translate for in the XDR space.

### Benefits

*   Any Direct Project sender can send to any XDR endpoint as long as there is a Trusted "Direct Project to XDR" Destination HISP service available to do the translation.
*   The XDR receivers have one way to receive directed messages regardless of whether the messages originate from inside their XDR network (e.g. NHIN Exchange) or from outside that network using the Direct Project.
*   There is no need for the XDR receivers to have independent digital certificates, although the Trusted Service could manage multiple individual digital certificates (even though there is little additional benefit, since all of them terminate in the one service).

### Drawbacks

*   The Trusted Service Provider has full access to PHI.
*   If the Direct Project package created by the sender is not XDM-based, then the minimal metadata definition will probably apply. Errors due to missing metadata must be handled, based on policy. This is the topic of the[ XDR and XDM for Direct Messaging](/XDR+and+XDM+for+Direct+Messaging) specification.

### Risk Assessment

 This Deployment Model Risk leverages the other threat models for the pathway within the Direct Project space, and the IHE XDR Risk Assessment for the pathway within the XDR space. The Risk Assessment for the conversion from Direct Project to XDR is found in [Threat Model - Direct to and from XDR](/Threat+Model+-+Direct+to+and+from+XDR).  

## E.2 Direct Project receiving from XDR sender using a Trusted Service Provider (e.g. NHIN Exchange)

### Preconditions![NHIN-Direct-Deployment-Models-E2-r2.png](/file/view/NHIN-Direct-Deployment-Models-E2-r2.png/173924425/640x480/NHIN-Direct-Deployment-Models-E2-r2.png "NHIN-Direct-Deployment-Models-E2-r2.png")

*   All Direct Project [Consensus Proposal - Assumptions](/Consensus+Proposal) are in play.
*   The sender and receiver have ensured that agents of the sender and receiver (for example, HIO, HISP, intermediary) are authorized to act as such and are authorized to handle protected health information according to law and policy.
*   Specifically, the XDR Sender is utilizing a "Direct Project from XDR" Source HISP as a trusted service provider that will sign the content and discover the recipient's certificate to be used for encryption.

### Benefits

*   Any XDR sender can send to any Direct Project receiver as long as there is a Trusted "Direct Project from XDR" service available to do the translation.
*   The XDR senders have only one way to send directed messages regardless of whether the messages are sent to addresses inside their XDR network (e.g. NHIN Exchange) or to addresses outside that network using the Direct Project.
*   There is no need for the XDR senders to have independent digital certificates, although the Trusted Service could manage multiple individual digital certificates (even though there is little additional benefit, since all of them terminate in the one service).
*   All XDR receivers send content that is XDR-compliant. This content is easily wrapped in XDM content packaging for delivery to Direct Project recipient. This is the topic of the[ XDR and XDM for Direct Messaging ](/XDR+and+XDM+for+Direct+Messaging)specification.

### Drawbacks

*   The Trusted Service Provider has full access to PHI

### Risk Assessment

 This Deployment Model Risk leverages the other threat models for the pathway within the Direct Project space, and the IHE XDR Risk Assessment for the pathway within the XDR space. The Risk Assessment for the conversion from Direct Project to XDR is found in [Threat Model - Direct to and from XDR](/Threat+Model+-+Direct+to+and+from+XDR).  

# References

 All the slides are in this Powerpoint Presentation  
 <div class="objectEmbed">[![NHIN-Direct-Deployment-Models-20101208.ppt](http://www.wikispaces.com/i/mime/32/empty.png)](/file/view/NHIN-Direct-Deployment-Models-20101208.ppt/186453849/NHIN-Direct-Deployment-Models-20101208.ppt)<div>[NHIN-Direct-Deployment-Models-20101208.ppt](/file/view/NHIN-Direct-Deployment-Models-20101208.ppt/186453849/NHIN-Direct-Deployment-Models-20101208.ppt "NHIN-Direct-Deployment-Models-20101208.ppt")  

*   [Details](/file/detail/NHIN-Direct-Deployment-Models-20101208.ppt)
*   [Download](/file/view/NHIN-Direct-Deployment-Models-20101208.ppt/186453849/NHIN-Direct-Deployment-Models-20101208.ppt)
*   689 KB

</div></div></div></div></div>
