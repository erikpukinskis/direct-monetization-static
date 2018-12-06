---
title: The 402-Receipt Proposed standard
date: 2018-12-06 12:00:00 Z
permalink: "/introduction.html"
description: The 402-Receipt Proposed standard.
---

#### Version negative one.

The 402 response code implies that a request is being denied because payment must be provided for the requested resource. No standard way of following up to a 402 response has been normalized, as the 402 response code has seen only niche usage, mostly for subscription-only APIs, and is effectively non-standard for human-facing http resources.

We propose a suite of http headers, html head elements, and surrounding utilities to make direct monetization of web-accessible resources low-stress and safe even for non-specialist human users.

By design, this standard covers use-cases in which a 402 response would never actually be sent. We can think of this like a tip-jar or crowd-funding tool; no payment is _required_ to access the resource, but a clear means of making optional payments is exposed to the client along with explicit expectations about what they _should_ pay.

Throughout this document we will talk about interchangeably about "requiring" or "accepting" receipts. The actual behavior gets specified in the Receipt Definition.

Although this standard is only applicable for HTTP exchanges; it's desirable that the framework of receipts translate clearly to other internet communications. 

## Context for a 402 response
It's generally poor user experience to reject a web-page request outright. A 402 response to such a request may work if a suitable placeholder page is available and the client is trusted to handle the response appropriately.

In practice, it is usually better to give normal responses to the principal request, and require receipts for a key resource used by that page, for example an image or text-block. To avoid performance impacts, a page should provide instructions about what receipts the secondary resource will require. **This information must be accurate to avoid a visitor paying for a useless receipt.**

## Some early concerns:
- The duplication of HTTP header specifications and in-line html feels weird, but is probably the way forward. In particular, in-line html is probably easier for a most website authors to implement and HTTP headers aren't accessible from JS, but HTTP headers feel like they'll have better separation of concerns, work with images, and are probably friendlier for robots.
- Having the signatories do their signatures blindly is highly desirable, but fundamentally incompatible with passive-host design. 

