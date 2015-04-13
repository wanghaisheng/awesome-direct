Evaluation Criteria for Trust Anchors and Certification Authorities

You are not a member of this wiki. Join now  Dismiss
guest| Join | Help | Sign In
Direct Project

    Wiki Home
    Recent Changes
    Pages and Files
    Members

    Home
    Events
        January 2015 Connect-A-Thon
        ONC Direct Boot Camp
    Implementation Group
    Direct Rules of the Road
        Direct Communities
    Workgroups:
        Best Practices
        Communications
        Implementation Geographies
        Reference Implementation
        Previous Workgroups
    Key Items:
        Documentation Library
        Design Principles
        User Stories
        Specifications and Service Descriptions
    Participation
        Guidelines for Participation
        Blog
        Discuss
        IPR Policy
    HHS NwHIN Overview
    FAQ
    Contact



Twitter-icon.png

Mention of any product or organization on these pages does not imply endorsement of that product or organization by ONC, HHS, or any other Federal agency.
Evaluation Criteria for Trust Anchors and Certification Authorities
Evaluation Criteria for Trust Anchors and Certification Authorities
This document is up for Implementation Group consensus at Evaluation Criteria for Trust Anchors and Certification Authorities - IG Consensus.

Document Purpose
Organizations implementing Direct specifications must configure trust anchors (or, through a Business Associate Agreement or similar contractual document, delegate responsibility to their full-service HISP) to ensure common trust with information exchange participants also using Direct specifications. The decision to add or not add a trust anchor has high consequence for the privacy and security of health information exchange. Adding a trust anchor that does not ensure identity assurance, security and key management that meets at least a common high bar of trust may compromise the integrity of the exchange.

This document describes the set of evaluation criteria an organization (the relying party) participating in exchange should apply to prospective trust anchors and the Certification Authorities (CAs) that supply them.

Objective Review Policies

The goal of the Direct Project is to enable broad interoperability at a transport level, using strong trust anchor policies to protect privacy, security and trust. Accordingly, as a best practice, organizations must develop strong trust anchor evaluation processes and apply them objectively to prospective trust anchors. Denial of prospective trust anchors must be based on these criteria and denials should be accompanied by the criterion violation that triggered the denial. Denial should not be based on subjective criteria, or on criteria not associated with protecting privacy, security and trust.

Certification Practice Statement
The operations of a CA are described in a Certification Practice Statement (CPS) (see RFC3647 for background on Certificate Practice Statement documents). The CPS is "a statement of the practices that a certification authority employs in issuing, managing, revoking, and renewing or re-keying certificates.." The CPS describes the internal practices of the CA, and covers certificates assigned for different purposes with different trust levels.

The relying party or delegate must ensure that the CPS meets good CA practice. Good CA practices have been defined by WebTrust, ETSI (TS 101456), and the Federal Bridge Certification Authority (FBCA)

CAs with the WebTrust seal, ETSI certification, or FBCA cross-certification have been validated to comply with good CA practices.

Certificate Policy
The policies by which certificates are assigned are documented in a Certificate Policy (CP) document (see RFC3647 for background on Certificate Policy documents). The CP is "a named set of rules that indicates the applicability of a certificate to a particular community and/or class of application with common security requirements. For example, a particular CP might indicate applicability of a type of certificate to the authentication of parties engaging in business-to-business transactions for the trading of goods or services within a given price range."

The CP associated with the trust anchor must meet applicable policies governing identity assurance and other security requirements for organizations or individuals participating in information exchange that are deemed appropriate by the relying party.

This document does not define those policies, but does point to some examples of well-known policies that may set appropriate floors for health information exchange.

Certificate Policy for Organizations
When certificates are assigned to organizations, rather than individual, there are two approaches that are often used:

    Verifying the identity of the organization
    Verifying the identity of an individual who in turn vouches for the identity of the organization


The requirements for issuance of Extended Validation certificates are a good example of strong identity assurance using the former approach, and the Federal Bridge Certification Authority CP is a good example of the latter approach.

Certificate Policy for Individuals
NIST Special Publication 800-63 defines common terminology for identity assurance of individuals; the Federal Bridge Certification Authority CP defines a CP with levels of identity assurance corresponding to certificate issuance mapping to the NIST levels. The defined levels of Basic (if NIST level 2 identity assurance is the desired floor) or Medium (for NIST level 3), as defined in the Federal Bridge Certification Authority CP, may be good reference points for good Certificate Policy statements

Validation of Trust Anchors
When receiving a root certificate that is to be used as a trust anchor, and at business-reasonable intervals thereafter, the relying party must validate the certificate by, at a minimum:

    Ensuring the certificate itself is a valid certificate that meets defined criteria for a CA certificate
    Checking the certificate expiry
    Checking that the certificate has not been revoked, using a CRL, OCSP or a similar mechanism
