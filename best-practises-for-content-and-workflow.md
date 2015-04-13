Best Practices for Content and Workflow
Overview

The goal of the Direct Project is to enable a wide network for directed health information exchange, with a backbone of universal addressing and universal transport, to improve health and health care. This goal requires a balancing act between two subordinate goals that often conflict:

    Opening exchange to a wide variety of exchange participants, including physicians who may not (yet) have complete electronic health records
    Enabling rich clinical workflows based on semantic interoperability with structured healthcare contents


The first goal argues for placing minimal barriers on content, to allow upgrade from fax and paper to electronic transactions. The second goal argues for restrictions on content to defined healthcare standards to enable rich clinical experience, cognitive support, and improved outcomes.

This document describes best practices to reconcile those two subordinate goals.

Lower Bounds On Information Exchange

To enable the broadest participation in information exchange, organizations that provide patient-centered clinical care must have at least one address that accepts the following bare content types:

    Plain text
    PDF
    Standard image types (JPEG, PNG)


Accepting these content types enables the distributed care team providing clinical care to the patient to participate in information exchange; restricting these content types forces such participants to use fax or paper transactions.

It must be recognized that accepting these content types (without content wrappers or accompanying structured content) is a compromise, and participants in exchange should be encouraged to upgrade to capabilities that enable exchange of structured information.

When sending, systems that are capable of sending structured information should enable receivers that only understand unstructured content types to view the received information. This may be achieved through, for example, alternative representations or transforms (e.g., PDF representations or XML stylesheets).

There are, of course, receivers that will intentionally have no business need or interest to receive unstructured data. For example, CMS may require, by regulation, that quality reports are submitted in PQRI XML format. In such cases, where the receiver never handles unstructured content; there is no need to handle the bare content types. In general, if the organization or business never handles fax or mailed data, there is no need to handle unstructured data electronically.

Enabling Rich Workflows

Structured content allows for rich clinical workflows, including associating content with the patient, enabling cognitive support aids (e.g., CDS, support for updating medication or problem lists, order or referral tracking, care planning, and the like). Enabling such workflows requires that the receiving system identify the received content and route it to the right pipeline for processing (including pipelines for unstructured content, that require pushing to a queue for human intervention).

Handling a variety of content types, including structured and unstructured content, makes selecting the right downstream pipeline potentially challenging.

There are a number of techniques that address this challenge:

    Special purpose addresses: Specific inbound addresses may have specific content expectations. For example, lab@direct.example.org may expect HL7 V2.5.1 ORU messages, and may error anything that does not conform to that expectation. This technique must be combined with a general purpose address (e.g., general@direct.example.org) that receives unstructured text.
    Content Detection: A general purpose address may test received content for specific patterns (e.g., HL7 V2, XML-based content types) and extract metadata needed for pipelining. This can be done based on the Content-Type metadata in the Direct base package, along with
    Use of XD* Metadata: XD* metadata provides for common properties that allow for workflow pipelining. Direct supports this metadata both through the native use of XDM packages and through "step up/down" from XDR. See the XDR and XDM for Direct Messaging specification for more detail.


These techniques may, of course be combined. The special purpose address patterns is most applicable to applications that "speak" the SMTP + S/MIME protocol natively; the XDM/XDR Rewrapping pattern is most applicable to applications that "speak" XDR natively, when combined with a Content Sniffing HISP.

2015 Edition - Health Information Technology Certification Criteria, Base Electronic Health Record Definition, and ONC Health IT Certification Program Modifications NPRM:
