---
swip: 0
title: SWIP Purpose and Guidelines
status: Active
type: Meta
author: Daniel A Nagy <daniel@ethswarm.org>, Tim Bansemer <tim@ethswarm.org>
created: 2019-07-08
updated: 2019-07-09
---

## What is an SWIP?

SWIP stands for Swarm Improvement Proposal. An SWIP is a design document providing information to the Swarm community, or describing a new feature for Swarm or its processes or environment. The SWIP should provide a concise technical specification of the feature and a rationale for the feature. The SWIP author is responsible for building consensus within the community and documenting dissenting opinions.

## SWIP Rationale

We intend SWIPs to be the primary mechanisms for proposing new features, for collecting community technical input on an issue, and for documenting the design decisions that have gone into Swarm. Because the SWIPs are maintained as text files in a versioned repository, their revision history is the historical record of the feature proposal.

For Swarm implementers, SWIPs are a convenient way to track the progress of their implementation. Ideally each implementation maintainer would list the SWIPs that they have implemented. This will give end users a convenient way to know the current status of a given implementation or library.

## SWIP Types

There are three types of SWIP:

- A **Standard Track SWIP** describes any change that affects most or all Swarm implementations, such as a change to the network protocol, a change in chunk format, file merkelization, manifest format, proposed application standards/conventions, or any change or addition that affects the interoperability of applications using Swarm. Furthermore Standard SWIPs can be broken down into the following categories. Standards Track SWIPs consist of three parts, a design document, implementation, and finally if warranted an update to the [formal specification].
  - **Core** - improvements requiring a network upgrade, data format changes, and the storer/node strategy changes.
  - **Networking** - includes improvements around [bzz/devp2p] and [Light Swarm Subprotocol], as well as proposed improvements to network protocol specifications.
  - **Interface** - includes improvements around client [API/RPC] specifications and standards, and also certain language-level standards like method names and [contract ABIs].
- A **Meta SWIP** describes a process surrounding Swarm or proposes a change to (or an event in) a process. Process SWIPs are like Standards Track SWIPs but apply to areas other than the Swarm protocol itself. They may propose an implementation, but not to Swarm's codebase; they often require community consensus; unlike Informational SWIPs, they are more than recommendations, and users are typically not free to ignore them. Examples include procedures, guidelines, changes to the decision-making process, and changes to the tools or environment used in Swarm development. Any meta-SWIP is also considered a Process SWIP.
- An **Informational SWIP** describes an Swarm design issue, or provides general guidelines or information to the Swarm community, but does not propose a new feature. Informational SWIPs do not necessarily represent Swarm community consensus or a recommendation, so users and implementers are free to ignore Informational SWIPs or follow their advice.

It is highly recommended that a single SWIP contain a single key proposal or new idea. The more focused the SWIP, the more successful it tends to be. A change to one client doesn't require an SWIP; a change that affects multiple clients, or defines a standard for multiple apps to use, does.

A SWIP must meet certain minimum criteria. It must be a clear and complete description of the proposed enhancement. The enhancement must represent a net improvement. The proposed implementation, if applicable, must be solid and must not complicate the protocol unduly.

## SWIP Work Flow

Parties involved in the process are you, the champion or *SWIP author*, the [*SWIP editors*](#SWIP-editors), and the [*Swarm Developers*](https://github.com/ethersphere/swarm-docs/team.md).

:warning: Before you begin, vet your idea, this will save you time. Ask the Swarm community first if an idea is original to avoid wasting time on something that will be rejected based on prior research (searching the Internet does not always do the trick). It also helps to make sure the idea is applicable to the entire community and not just the author. Just because an idea sounds good to the author does not mean it will work for most people in most areas where Swarm is used. Examples of appropriate public forums to gauge interest around your SWIP include, [the Issues section of this repository], and [one of the Swarm Mattermost chat rooms](http://beehive.ethswarm.org) or [the discussion forum](https://swarmresear.ch/). In particular, [the Issues section of this repository] is an excellent place to discuss your proposal with the community and start creating more formalized language around your SWIP.

Your role as the champion is to write the SWIP using the style and format described below, shepherd the discussions in the appropriate forums, and build community consensus around the idea. Following is the process that a successful SWIP will move along:

```
[ WIP ] -> [ DRAFT ] -> [ LAST CALL ] -> [ ACCEPTED ] -> [ FINAL ]
```

Each status change is requested by the SWIP author and reviewed by the SWIP editors. Use a pull request to update the status. Please include a link to where people should continue discussing your SWIP. The SWIP editors will process these requests as per the conditions below.

* **Active** -- Some Informational and Process SWIPs may also have a status of “Active” if they are never meant to be completed. E.g. SWIP 0 (this SWIP).
* **Work in progress (WIP)** -- Once the champion has asked the Swarm community whether an idea has any chance of support, they will write a draft SWIP as a [pull request]. Consider including an implementation if this will aid people in studying the SWIP.
  * :arrow_right: Draft -- If agreeable, SWIP editor will assign the SWIP a number (generally the issue or PR number related to the SWIP) and merge your pull request. The SWIP editor will not unreasonably deny an SWIP.
  * :x: Draft -- Reasons for denying draft status include being too unfocused, too broad, duplication of effort, being technically unsound, not providing proper motivation or addressing backwards compatibility and Swarm's mission.
* **Draft** -- Once the first draft has been merged, you may submit follow-up pull requests with further changes to your draft until such point as you believe the SWIP to be mature and ready to proceed to the next status. An SWIP in draft status must be implemented to be considered for promotion to the next status (ignore this requirement for core SWIPs).
  * :arrow_right: Last Call -- If agreeable, the SWIP editor will assign Last Call status and set a review end date (`review-period-end`), normally 14 days later.
  * :x: Last Call -- A request for Last Call status will be denied if material changes are still expected to be made to the draft. We hope that SWIPs only enter Last Call once, so as to avoid unnecessary noise.
* **Last Call** -- This SWIP will listed on the Swarm website (Open - Link TBD)
  * :x: -- A Last Call which results in material changes or substantial unaddressed technical complaints will cause the SWIP to revert to Draft.
  * :arrow_right: Accepted (Core SWIPs only) -- A successful Last Call without material changes or unaddressed technical complaints will become Accepted.
  * :arrow_right: Final (Not core SWIPs) -- A successful Last Call without material changes or unaddressed technical complaints will become Final.
* **Accepted (Core SWIPs only)** -- This SWIP is in the hands of the Swarm client developers.  Their process for deciding whether to encode it into their clients as part of the SWIP process.
  * :arrow_right: Final -- Standards Track Core SWIPs must be implemented and demonstrated in a test network. When the implementation is complete and adopted by the community, the status will be changed to “Final”. We aspire to have a second client implementation for the Swarm specification, at which point interoperability with both clients becomes required.
* **Final** -- This SWIP represents the current state-of-the-art. A Final SWIP should only be updated to correct errata.

Other exceptional statuses include:

* **Deferred** -- This is for core SWIPs that have been put off for a future network.
* **Abandoned** -- This SWIP is no longer pursued by the original authors or it may not be a (technically) preferred option anymore.
* **Rejected** -- An SWIP that is fundamentally broken or a Core SWIP that was rejected by the Core Devs and will not be implemented.
* **Active** -- This is similar to Final, but denotes an SWIP which may be updated without changing its SWIP number.
* **Superseded** -- An SWIP which was previously final but is no longer considered state-of-the-art. Another SWIP will be in Final status and reference the Superseded SWIP.

## What belongs in a successful SWIP?

Each SWIP should have the following parts:

- Preamble - RFC 822 style headers containing metadata about the SWIP, including the SWIP number, a short descriptive title (limited to a maximum of 44 characters), and the author details. 
- Simple Summary - “If you can’t explain it simply, you don’t understand it well enough.” Provide a simplified and layman-accessible explanation of the SWIP.
- Abstract - a short (~200 word) description of the technical issue being addressed.
- Motivation (*optional) - The motivation is critical for SWIPs that want to change the Swarm protocol. It should clearly explain why the existing protocol specification is inadequate to address the problem that the SWIP solves. SWIP submissions without sufficient motivation may be rejected outright.
- Specification - The technical specification should describe the syntax and semantics of any new feature. The specification should be detailed enough to allow competing, interoperable implementations for the current Swarm platform and later implementations thereof.
- Rationale - The rationale fleshes out the specification by describing what motivated the design and why particular design decisions were made. It should describe alternate designs that were considered and related work, e.g. how the feature is supported in other languages. The rationale may also provide evidence of consensus within the community, and should discuss important objections or concerns raised during discussion.
- Backwards Compatibility - All SWIPs that introduce backwards incompatibilities must include a section describing these incompatibilities and their severity. The SWIP must explain how the author proposes to deal with these incompatibilities. SWIP submissions without a sufficient backwards compatibility treatise may be rejected outright.
- Test Cases - Test cases for an implementation are mandatory for SWIPs that are affecting breaking changes. Other SWIPs can choose to include links to test cases if applicable.
- Implementations - The implementations must be completed before any SWIP is given status “Final”, but it need not be completed before the SWIP is merged as draft. While there is merit to the approach of reaching consensus on the specification and rationale before writing code, the principle of “rough consensus and running code” is still useful when it comes to resolving many discussions of API details.
- Copyright Waiver - All SWIPs must be in the public domain. See the bottom of this SWIP for an example copyright waiver.

## SWIP Formats and Templates

SWIPs should be written in [markdown] format.
Image files should be included in a subdirectory of the `assets` folder for that SWIP as follows: `assets/SWIP-X` (for SWIP **X**). When linking to an image in the SWIP, use relative links such as `../assets/SWIP-X/image.png`.

## SWIP Header Preamble

Each SWIP must begin with an [RFC 822](https://www.ietf.org/rfc/rfc822.txt) style header preamble, preceded and followed by three hyphens (`---`). This header is also termed ["front matter" by Jekyll](https://jekyllrb.com/docs/front-matter/). The headers must appear in the following order. Headers marked with "*" are optional and are described below. All other headers are required.

` SWIP:` <SWIP number> (this is determined by the SWIP editor)

` title:` <SWIP title>

` author:` <a list of the author's or authors' name(s) and/or username(s), or name(s) and email(s). Details are below.>

` * discussions-to:` \<a url pointing to the official discussion thread\>

` status:` <Draft | Last Call | Accepted | Final | Active | Abandoned | Deferred | Rejected | Superseded>

`* review-period-end:` <date review period ends>

` type:` <Standards Track (Core, Networking, Interface)  | Informational | Meta>

` * category:` <Core | Networking | Interface>

` created:` <date created on>

` * updated:` <comma separated list of dates>

` * requires:` <SWIP number(s)>

` * replaces:` <SWIP number(s)>

` * superseded-by:` <SWIP number(s)>

` * resolution:` \<a url pointing to the resolution of this SWIP\>

Headers that permit lists must separate elements with commas.

Headers requiring dates will always do so in the format of ISO 8601 (yyyy-mm-dd).

#### `author` header

The `author` header optionally lists the names, email addresses or usernames of the authors/owners of the SWIP. Those who prefer anonymity may use a username only, or a first name and a username. The format of the author header value must be:

> Random J. User &lt;address@dom.ain&gt;

or

> Random J. User (@username)

if the email address or GitHub username is included, and

> Random J. User

if the email address is not given.

#### `resolution` header

The `resolution` header is required for Standards Track SWIPs only. It contains a URL that should point to an email message or other web resource where the pronouncement about the SWIP is made.

#### `discussions-to` header

While an SWIP is a draft, a `discussions-to` header will indicate the mailing list or URL where the SWIP is being discussed. As mentioned above, examples for places to discuss your SWIP include [Swarm topics on Mattermost](https://beehive.ethswarm.org/) and the [Swarm Research Forum](https://swarmresear.ch/) or this repo.

No `discussions-to` header is necessary if the SWIP is being discussed privately with the author.

As a single exception, `discussions-to` cannot point to GitHub pull requests.

#### `type` header

The `type` header specifies the type of SWIP: Standards Track, Meta, or Informational. If the track is Standards please include the subcategory (core, networking, interface, or ERC).

#### `category` header

The `category` header specifies the SWIP's category. This is required for standards-track SWIPs only.

#### `created` header

The `created` header records the date that the SWIP was assigned a number. Both headers should be in yyyy-mm-dd format, e.g. 2001-08-14.

#### `updated` header

The `updated` header records the date(s) when the SWIP was updated with "substantial" changes. This header is only valid for SWIPs of Draft and Active status.

#### `requires` header

SWIPs may have a `requires` header, indicating the SWIP numbers that this SWIP depends on.

#### `superseded-by` and `replaces` headers

SWIPs may also have a `superseded-by` header indicating that an SWIP has been rendered obsolete by a later document; the value is the number of the SWIP that replaces the current document. The newer SWIP must have a `replaces` header containing the number of the SWIP that it rendered obsolete.

## Auxiliary Files

SWIPs may include auxiliary files such as diagrams. Such files must be named SWIP-XXXX-Y.ext, where “XXXX” is the SWIP number, “Y” is a serial number (starting at 1), and “ext” is replaced by the actual file extension (e.g. “png”).

## Transferring SWIP Ownership

It occasionally becomes necessary to transfer ownership of SWIPs to a new champion. In general, we'd like to retain the original author as a co-author of the transferred SWIP, but that's really up to the original author. A good reason to transfer ownership is because the original author no longer has the time or interest in updating it or following through with the SWIP process, or has fallen off the face of the 'net (i.e. is unreachable or isn't responding to email). A bad reason to transfer ownership is because you don't agree with the direction of the SWIP. We try to build consensus around an SWIP, but if that's not possible, you can always submit a competing SWIP.

If you are interested in assuming ownership of an SWIP, send a message asking to take over, addressed to both the original author and the SWIP editor. If the original author doesn't respond to email in a timely manner, the SWIP editor will make a unilateral decision (it's not like such decisions can't be reversed :)).

## SWIP Editors

The current SWIP editors are

` * Daniel A. Nagy <daniel@ethswarm.org>`

` * Elad Nachmias <elad@ethswarm.org>`

## SWIP Editor Responsibilities

For each new SWIP that comes in, an editor does the following:

- Read the SWIP to check if it is ready: sound and complete. The ideas must make technical sense, even if they don't seem likely to get to final status.
- The title should accurately describe the content.
- Check the SWIP for language (spelling, grammar, sentence structure, etc.), markup (Github flavored Markdown), code style

If the SWIP isn't ready, the editor will send it back to the author for revision, with specific instructions.

Once the SWIP is ready for the repository, the SWIP editor will:

- Assign an SWIP number (generally the PR number or, if preferred by the author, the Issue # if there was discussion in the Issues section of this repository about this SWIP)

- Merge the corresponding pull request

- Send a message back to the SWIP author with the next step.

Many SWIPs are written and maintained by developers with write access to the Swarm codebase. The SWIP editors monitor SWIP changes, and correct any structure, grammar, spelling, or markup mistakes we see.

The editors don't pass judgment on SWIPs. We merely do the administrative & editorial part.

## History


This document was derived heavily from [Ethereum's EIP-1]. The SWIP process was created on the basis of the [EIP process](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-1.md). [EIP-1] is based on [Bitcoin's BIP-0001] written by Amir Taaki which in turn was derived from [Python's PEP-0001]. In many places text was simply copied and modified. Although the PEP-0001 text was written by Barry Warsaw, Jeremy Hylton, and David Goodger, they are not responsible for its use in the SWarm Improvement Process, and should not be bothered with technical questions specific to Swarm or the SWIP. Please direct all comments to the SWIP editors.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

