
# Introduction

 This page is intended to model threats associated with a conversation between an endpoint that is using a classic Direct Project deployment and an operational environment using XDR. Where the classic Direct Project is represented by any of the other [Deployment Models](/Deployment+Models). The specific Deployment Model is described [here](/Deployment+Models#Deployment Models-E. Direct Project to/from XDR (e.g., NHIN Exchange)). Although messages may contain multiple recipients, we have constrained this model to only one for simplicity. There are of course a number of other possible configurations that alter the threat profile; these will be evaluated on separate pages as appropriate.

 This threat assessment followed the [Threat Model Process](/Threat+Model+Process). Other Deployment Models have been similarly assessed and are cataloged on the [Threat Models](/Threat+Models) page.
 The threat assessment on this page includes only the translation to or from the Direct Project specification to or from the XDR specification. For the Threat Model for the classic Direct Project sender or receiver see the other [Threat Models](/Threat+Models). The Threat Assessment for the XDR health information exchange is identified in the IHE XDR specification.  

 This Threat Assessment leverages the other Direct Project Threat Models to cover the pathway within the Direct Project space, and the IHE XDR Risk Assessment for the pathway within the XDR space. This Risk Assessment will cover only the risks around the trusted service provider that is doing the conversion between the Direct Project and XDR.  

## E. Direct Project to/from XDR (e.g., NHIN Exchange)

 For the Direct Project to/from XDR push based system (e.g. NHIN Exchange) Deployment Model I split this up into two diagrams for readability. I treat the Direct Project as a cloud and the XDR environment (e.g., NHIN Exchange) as another cloud showing only the steps needed to bridge between them. With this deployment model there is the need to treat the XDR environment as a fully trusted party. While the XDR environment may be an NHIN Exchange participant, it could also be another entity that is not in NHIN Exchange, such as an HIO (local, regional, state) or an EHR that communicates via XDR.  

 This deployment model is converting XDR to and from The Direct Project specification, there is special handling of this conversion of transport, content packaging, and metadata. This is the topic of the [XD* Conversions for Direct Messaging](/XD%2A+Conversions+for+Direct+Messaging) specification.  

 A system in an XDR environment can always choose to also be a Direct Project participant to get end-to-end security. The choice should be based on the Sender and Receiver relationship and the Risks Management they deploy. A solution supporting both XDR and The Direct Project specification would be an application that can decide which transport to use to get to the desired destination.

### Note on MDN "Read Receipts"

 The Direct Project specification formalizes the use of the Message Disposition Notification ("MDN") mechanism for confirming receipt of a message. At this time the MDN is not modeled for how it will be supported with this deployment model, so it is not assessed here.  

## E.1 Direct Project sending to XDR with Trusted Service Provider (e.g. NHIN Gateway)

 The Pre-Conditions, Benefits and Drawbacks are outlined [here](/Deployment+Models#Deployment Models-E.1 Direct Project sending to XDR with Trusted Service Provider (e.g. NHIN Gateway)) in the [Deployment Models](/Deployment+Models).  

# Diagram:

![NHIN-Direct-Deployment-Models-E1-r2.png](/file/view/NHIN-Direct-Deployment-Models-E1-r2.png/173924303/640x480/NHIN-Direct-Deployment-Models-E1-r2.png "NHIN-Direct-Deployment-Models-E1-r2.png")  

## E.2 Direct Project receiving from XDR sender using a Trusted Service Provider (e.g. NHIN Gateway)

 The Pre-Conditions, Benefits and Drawbacks are outlined [here](/Deployment+Models#Deployment Models-E.3 Direct Project receiving from XDR sender using a Trusted Service Provider (e.g. NHIN Gateway)) in the [Deployment Models](/Deployment+Models).  

# Diagram:

 ![NHIN-Direct-Deployment-Models-E2-r2.png](/file/view/NHIN-Direct-Deployment-Models-E2-r2.png/173924425/640x480/NHIN-Direct-Deployment-Models-E2-r2.png "NHIN-Direct-Deployment-Models-E2-r2.png")  

# Risk Assessment and Mitigation Plan

 In the interests of keeping scope on the Risk Assessment: We assume good programming practices and good business operational practices. For example we do not include risks of poor protocol implementation that would expose buffer-overflows, or poor operational environment that allows sharing of accounts, key-loggers, and openly readable audit log files.  

 This Threat Assessment leverages the other Direct Project [Threat Models](/Threat+Models) to cover the pathway within the Direct Project space (Arc E.1.1 and E.2.9), and the IHE XDR Risk Assessment for the pathway within the XDR space (Arc E.1.7 and E.2.1). This Risk Assessment will cover only the risks around the trusted service provider that is doing the conversion between the Direct Project and XDR. Therefore the total Risk Assessment is the combination of all the risks found in the Direct Project, IHE XDR, and this risk assessment.  

 Where risks are the same with the other Threat Models we use the same Risk Number. The Mitigation is sometimes different given the Deployment Model.  

<table class="wiki_table"><tbody><tr><td>**Risk Nunber**  
</td><th>Arc  
</th><th>Risk  
</th><th>**Likelihood**  
 **(L, M, H)**  
</th><th>Impact  
 (L, M, H)  
</th><th>Mitigation  
</th><th>Notes  
</th></tr><tr><td>20  
</td><td>E.1.4  
 E.1.6  
 E.1.7  
 E.2.1  
 E.2.2  
 E.2.3  
 E.2.5  
 E.2.7  
</td><td>potential exposure of TO/FROM routing information inside the Gateway. Exposing that the addressee identified in the TO is having a private conversation with the addressee identified by the FROM.  

*   provider-to-provider -- There is no knowledge of the topic of the conversation, it could be golf.
*   provider-to-patient -- There is knowledge of types of conversations where the provider is a specialist

</td><td>L  
</td><td>L  
</td><td>The Gateway is a known entity that is explicitly considered. The Gateway should be carefully managed and audited.  
</td></tr><tr><td>21  
</td><td>E.1.4  
 E.1.6  
 E.1.7  
 E.2.1  
 E.2.2  
 E.2.3  
 E.2.5  
 E.2.7  
</td><td>If the trusted service is compromised, an internal or external attacker could steal messages held in queues.  
</td><td>L  
</td><td>H  
</td><td>The simplest mitigation for this attack is to use system-provided mechanisms to encrypt the at-rest message stores. This will not completely defeat an internal attacker, but will greatly reduce the vulnerable surface.  
 .  
</td><td>Assessed as Low probability based on the fact that the service is known by the CE as a sensitive resource.  
</td></tr><tr><td>22  
</td><td>E.1.3  
 E.2.6  
</td><td>The trusted service provider has access to the private keys of the sender and receiver and thus can decrypt anything sent to these endpoints and can impersonate any of these endpoints  
</td><td>L  
</td><td>H  
</td><td>The Gateway is a known entity that is explicitly considered. The Gateway should be carefully managed and audited.  

 The XDR endpoints could all use the same Certificate and thus private key. Thus the encryption is targeted to the gateway and messages are signed by the gateway.  
</td></tr><tr><td>23  
</td><td>E.1.4  
 E.1.6  
 E.1.7  
 E.2.1  
 E.2.2  
 E.2.3  
 E.2.5  
 E.2.7  
</td><td>If the trusted service is compromised, an internal or external attacker could modify or falsify content that would be undetectable.  
</td><td>L  
</td><td>H  
</td><td>The problem we have is that the S/MIME signature is very specific to the S/MIME packaging, WS-Security signature is specific to SOAP. So neither end-to-end security system can be preserved across this end-to-end conversation. However if someone really needs end-to-end signature, they can either  
 a) Use a Document Digital Signature, such as IHE DSG Profile outlines. These are designed to be persistent as well as other useful attributes.  
 b) Use NHIN Direct end-to-end… meaning that someone on XDR network would ignore the XDR pathway and go S/MIME direct from their desktop to the receiving individual’s desktop. Meaning they are NOT using this deployment model (E) but rather using deployment model (B).  

 All other deployment models trade off complexity at the user end for security.  
</td></tr><tr><td>24  
</td></tr></tbody></table></div></div></div>
