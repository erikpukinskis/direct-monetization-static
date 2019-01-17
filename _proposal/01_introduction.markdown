---
title: The 402-Receipt proposed standard
date: 2018-12-06 12:00:00 Z
permalink: "/introduction.html"
description: The 402-Receipt Proposed standard.
documentation_order: 1
---

# Version negative one.
#### Goal of this document:
This document describes a standard way for a website to demand or accept payment for access to resources without having a prior relationship with the customer or at any point collecting any card/bank information. The proposed standard is a suite of HTTPS headers, HTML head elements, and surrounding utilities to make direct monetization of web-accessible resources low-stress and safe even for non-specialist human users.

The standard needs to be sufficiently defined that tools within it can be built independently and work together. It must be a suitable tool for producers and consumers, so if you have questions, ideas, or suggestions about the technical details or about the high-level user experience then please reach out to us. 

## How to help
This website is on [GitHub](https://github.com/ShapeOfMatter/direct-monetization-static); pull requests are welcome. You can also email us with questions or suggestions at <dmn@directmonetization.network>.
We'll move to our own website an domain as soon as we're sure we have a good name.

## The name
The 402 HTTPS response code implies that a request is being denied because payment must be provided for the requested resource. [No standard way of following up to a 402 response has been normalized](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#402). The 402 response code has seen only niche usage, mostly for subscription-only APIs, and is effectively non-standard for human-facing HTTPS resources. This standard will fix that.

### Consumer tools:
We anticipate a distinction between the [Client]({{ "/client.html" | relative_url }}) (typically a web-browser, possibly with a plugin) and a remote [Wallet]({{ "/client.html" | relative_url }}) which will allow persistence of user-data between clients and devices.

### Producer tools:
Different [Hosts]({{ "/host.html" | relative_url }}) will have different needs. The textbook usage will be a server plugin, designated servlet, or specialized CDN, that will inspect requests for receipts, validate the receipts, and forward them to their respective Notaries for disbursement. 

### Brokers and Networks:
Throughout this standard we speak of a [Notary]({{ "/notary.html" | relative_url }}); the Notary's role is not just to sign receipts; they also handle the collection of money from consumers and disbursement of it to producers. In the typical case both the host and the client will have a prior relationship (membership or account) set up with one or more Notary; as long as they have one in common the user experience can proceed seamlessly.

## Some early concerns:
- The duplication of HTTPS header specifications and in-line HTML feels weird, but is probably the way forward. In particular, in-line HTML is probably easier for most website authors to implement and HTTPS headers aren't accessible from JS, but HTTPS headers feel like they'll have better separation of concerns, work with images, and are probably friendlier for robots.
- Having the signatories do their signatures blindly is highly desirable, but fundamentally incompatible with passive-host design. 

