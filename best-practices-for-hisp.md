Best Practices for HISPs
Status

    Consensus was achieved by the full Implementation Group and by the Best Practices Workgroup


Overview

The term Health Information Service Provider (HISP) has been used by the Direct project both to describe a function (the management of security and transport for directed exchange) and an organizational model (an organization that performs HISP functions on behalf of the sending or receiving organization or individual). In this best practice document, we are mainly concerned with the HISP organization and the implications for privacy, security and transparency when the HISP is a separate business entity from the sending or receiving organization.

When the HISP functions are wholly contained within the organizational boundaries of a HIPAA Covered Entity, the issues discussed in this document do not generally apply, because the data use, retention, and disclosure decisions are made by the Covered Entity itself, under the full protections of HIPAA. It is when a separate business entity exists that it is particularly important to clarify the privacy, security and transparency requirements of that separate entity.

This document describes some of the key best practices required to ensure that:

    Individuals, providers and provider organizations can participate in directed information exchange with confidence
    Contracting and legal agreements can scale without requiring central DURSA-like arrangements (e.g., such that providers need only one legal agreement with their HISP, and such that HISPs need not mutually contract to protect privacy and security).


As with all best practice documents, this document covers some ground being considered by HIT Policy Committee (HITPC) workgroups. These include, in particular, the Privacy and Security Tiger Team and the Governance Workgroup. The intent is to harmonize this document over time with the final recommendations of the HITPC.

HIPAA and Legal Agreements

Because the HISP is a separate business organization from the sending and receiving organization, there are some functions that should be enforced through contract law, in addition to those enforced by Federal, state and local laws and regulations.

In many cases, HISPs will be required to have Business Associate Agreements (BAAs) with HIPAA Covered Entities. In cases where BAAs are not strictly required (for example, because the sender or receiver is not a Covered Entity), coverage under similar agreements is essential to provide the equivalent safeguards and protections provided under contractual law to those provided by BAAs for Business Associates of Covered Entities.

Pilot Recommendation: All HISPs must have contractually binding legal agreements with the sender or receiver of directed exchange of Personally Identifiable Information (PII), including all terms and conditions required in a BAA and including other protections as noted in this best practice.

HIPAA provides legal safeguards and clear requirements for Individuals (patients/consumers) and Covered Entities, as defined in HIPAA. There are some participants that will desire to participate in directed exchange but will not meet the legal triggers for Covered Entity status. Including such participants in directed exchange without ensuring the same legal safeguards provided under HIPAA is problematic. HIPAA extends privacy safeguards and protections to apply to Business Associates of Covered Entities. Directed exchange of PII that involves intermediaries or third parties that are not Business Associates or covered by equivalent protections is likewise problematic, unless separate mutual contracts are in place to protect privacy, security and transparency. Because a model that requires mutual contracting is not operationally scalable, it is desirable to limit exchange to entities that have clear recognized responsibilities under HIPAA.

Exchange of data protected by strong encryption over the open Internet using pure routing functions (e.g., TCP/IP switching, SMTP servers handing encrypted data) generally does not need these levels of protection so long as the routing organizations do not have access to the decryption keys.

Pilot Recommendation: Directed exchange where participants have access to unencrypted data or could have access to unencrypted data (because they hold decryption keys for encrypted data) must involve Individuals, Covered Entities and Business Associates (using those terms as defined by HIPAA) OR organizations that have strong legally enforceable contractual obligations that provide equivalent protection for individuals to those provided by HIPAA.

By noting "equivalent protection," we recognize that the investigational powers and remedies available in contractual law do not equal those available to the Federal Government under HIPAA.

Security

The ability to protect PHI through strong information security is critical to establishing trust in directed messaging. There are well-established best practices in information security across multiple industry contexts that are available for HISPs to follow.

The HIPAA Security Rule establishes uniform national standards for information security. The security rule applies to Covered Entities, and, by extension to Business Associates; Meaningful Use also requires compliance to the HIPAA Security Rule. At a more detailed level, information security guidelines have been developed to protect cardholder data for financial transactions, in the PCI Data Security Standards (PCI-DSS); many of the detailed requirements for that purpose have broad applicability to health care as well.

Pilot Recommendation: Regardless of legal requirements, all HISPs will hold themselves to the provision of the HIPAA Security Rule, and, to the extent that it is relevant and consistent with the Security Rule, will follow the guidelines of PCI-DSS.

Some HISPs will maintain private keys as well as public certificates, and will use those private keys to manage trust on behalf of providers, provider organizations, or other participants in health information exchange. This implies a high degree of trust that must be protected with a high degree of security.

Pilot Recommendation: HISPs that manage private keys must perform specific risk assessment and risk mitigation to ensure that the private keys have the strongest protection from unauthorized use. That risk assessment must address the risk of internal personnel or external attackers gaining unauthorized access either to the keys or to the health information functions for which the keys enforce trust.

Some HISPs will manage trust anchors on behalf of their customers. This is a critical aspect of trust, and must be managed well to avoid compromise on strong assurance of identity.

Pilot Recommendation: HISPs that manage trust anchors on behalf of their customers must have well defined, publicly available policies for evaluating the certificate issuance policies of those trust anchors, in accordance with the Certificate Pilot Recommendations

Transparency and Data Handling/Retention

Any HISP that manages, transforms, performs value-added services, or otherwise handles PHI has an obligation to make the extent of data use, retention, data handling, and other activities clear and transparent.

The Privacy and Security Tiger Team has published recommendations on transparency on their approved recommendation to the HIT Policy Committee. Those recommendations cover, in recommendation #1, specific recommendations to include in BAAs or service agreements "how they use and disclose information, including without limitation their use and disclosure of de-identified data, their retention policies and procedures, and their data security practices." Given that even support of the most basic forms of directed messaging may require data retention and use in support of security, audit, logging, or other required operational functions, it is important that transparency apply to those functions as well.

The same recommendations define the criteria for "directed exchange", and make it clear that absent other data retention and use triggers, directed exchange does not require enhanced patient consent. We believe that when properly implemented to implement directed exchange, the Direct specifications fall cleanly within the Tiger Team recommendations.

The Direct specifications can also be used for purposes that do not fall within the Tiger Team definition of "directed exchange." For instance, such uses would include the use of a directed push to a repository which is then used for subsequent queries. If a HISP implements additional functionality using the Direct standards which goes beyond "directed exchange," then obtaining additional (enhanced) consent may be required of the users of such services. If a HISP also implements services which require "enhanced consent," it is important that the Direct implementation segregate data use and retention services such that users of Direct for directed exchange do not invoke the need for enhanced consent.

Pilot Recommendations: HISPs must include all data collection, use, retention and disclosure policies (including rights reserved but not exercised) in BAAs or other service agreements.

Pilot Recommendations: HISPs must minimize data collection, use, retention and disclosure to that minimally required to meet the level of service required of the HISP.

Pilot Recommendations: Minimal use may require retention of data for security, audit, logging and other required operation; such use must be included in BAAs and service agreements, and must capture the minimal amount of data to fulfill those requirements.

Pilot Recommendations: To the extent that HISPs support multiple functions with different requirements for data use, they must separate those functions such that more extensive data use or disclosure is not required for more basic exchange models. 
