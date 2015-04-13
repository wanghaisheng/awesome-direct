Best Practices for HISP-HISP Agreements
Overview

As noted in Best Practices for HISPs, a key goal of the design of Direct specifications is to ensure "contracting and legal agreements can scale without requiring central DURSA-like arrangements (e.g., such that providers need only one legal agreement with their HISP, and such that HISPs need not mutually contract to protect privacy and security."

This document provides the rationale for that statement and proposes a best practice of free HISP-HISP interchange without the requirement for pre-existing agreements between HISPs. Although it provides factual evidence for the best practice, it is not a legal opinion or legal guidance.

Trust and Privacy Considerations

The Direct Project specifications were designed to address the following preconditions of trust:

    The sender has assurance that the receiver is who the receiver purports to be
    The receiver has the same level of assurance in the sender
    Both have assurance that the content will not be modified in transit
    Exposure to personally identifiable information (PII) or protected health information (PHI) is under the complete control of sender and receiver


These preconditions are assured through the use of S/MIME, in which messages are signed with the sender's private key and encrypted with the receiver's public key. Because of this:

    The sender ensures that only intended receiver can view the content (through use of the receiver's private key to decrypt the data)
    The receiver ensure that the content is as was sent by the sender (through the use of the sender's signature)
    Both parties ensure that they trust the identity assurance and other certificate issuance policies of the sender and receiver's certification authority.


The choice of appropriate trust anchors is essential to achieve these preconditions (see Evaluation Criteria for Trust Anchors and Certification Authorities).

Deployment Models And Associated PHI/PII Disclosure Considerations

The deployment models associated with use of Direct Project specifications allow, under control of the sender and receiver, for the locus of encryption/decryption activities to take place either at the sender or receiver or with a separate party under an appropriate contractual relationship with the sender or receiver.

The basic case is one in which sender and receiver take sole responsibility for encryption/decryption activities. Access to unencrypted PHI/PII is confined to the sender and receiver; only encrypted content is sent outside of the organizational confines of the sender or receiver. In this model, any intermediary is used purely for "electronic courier" services as noted by HHS OCR, particularly as the data associated is encrypted. There is, additionally, no breach risk in the transfer of the encrypted package, as any parties other than the sender or receiver are exposed to data that are "rendered unusable, unreadable, or indecipherable to unauthorized individuals" (HHS OCR HIPAA Breach Guidance) through the use of the NIST-recognized AES128 or AES256 encryption algorithms.

Either the sender or receiver may decide to delegate responsibility, under the terms of a BAA or equivalent contractual relationship, to a third-party, by letting the third-party manage keys on the sender or receiver's behalf. This action is entirely under the control of the sender or receiver, and may be reversed at any time (by moving the locus of encryption/decryption within the boundaries of the sender or receiver). Critically, from the perspective of the sender, a receiver taking sole responsibility for decryption is indistinguishable from a receiver who has delegated (via a BAA or equivalent) responsibility for this activity to a third-party. In either case, the sender or delegate hands off an encrypted package that can only be opened by the receiver to the IP address designated by the receiver, and the receiver (or delegate) receives the encrypted package. Breach can only occur before the package is encrypted (in which case, the data is under the control of the sender or the sender's BA) or after the package is decrypted (in which case, the data is under the control of the receiver or the receiver's BA).

In all cases or permutations, the handoff is always of a fully encrypted data package that is rendered indecipherable to unauthorized individuals. In the case where both sender and receiver delegate access to a full service HISP, the sending HISP has access to unencrypted content only through the BAA-authorized relationship between sender and sending HISP; the receiving HISP has access to decrypted content only through a similar BAA-authorized relationship to the receiver. The two HISPs never expose unencrypted PHI/PII to one another.

Negative Implications of HISP to HISP Contractual Preconditions for Transfer

As noted above, the sending entity (sender or sending HISP) hands off an encrypted package to the IP address specified by the receiver. The sending entity has no ability to know if the receiving entity is the actual receiver or the receiver's delegate. Likewise, the receiving entity has no ability to know if the sending entity is the actual sender or the sender's delegate (or an intermediary that merely relayed the encrypted package).

Also as noted, the locus of encryption/decryption is under the control of the sender/receiver and may be changed at any time. The sender can decide to move encryption/decryption inside the sending business entity, to move it to a BAA-covered third party, to change third-parties, without needing to inform receivers; the receiver can, of course, make the same decisions independent of the sender.

Therefore, a decision to require contractual relationships between HISPs as a precondition to send or receive encrypted data is effectively a decision to sign BAAs with every individual sender or receiver, which is to say, every covered entity, state entity, PCHR provider, and any other valid originator or recipient of health information.

(This, of course, is not to discourage HISPs from entering into contractual relationships with each other for the purposes of ensuring quality of service, uptime or other performance measures; just to discourage making those contractual relationships preconditions for exchange of encrypted data).

Conclusion

Following the Direct Project specifications, under all deployment models, sending and receiving entities only transmit data which has been rendered indecipherable to unauthorized individuals, through the combination of standard use of S/MIME (signing and encryption) and the specification of NIST-recognized strong encryption algorithms. Because of this, and because of the negative consequences of requiring contractual preconditions for transfer, there should be no need for HISPs to require contractual relationships as a precondition for exchange using Direct Project compliant implementations. 
