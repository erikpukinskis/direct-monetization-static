---
title: The 402-Receipt Proposed standard
date: 2018-12-06 12:00:00 Z
permalink: "/introduction.html"
description: The 402-Receipt Proposed standard.
documentation_order: 1
---

#### Goal of this document:
This document describes a standard way for a website to demand or accept payment for access to resources without having a prior relationship with the customer or at any point collecting any card/bank information. The standard needs to be sufficiently defined that tools within it can be built independently and work together. 

# Version negative one.

The 402 HTTPS response code implies that a request is being denied because payment must be provided for the requested resource. No standard way of following up to a 402 response has been normalized. The 402 response code has seen only niche usage, mostly for subscription-only APIs, and is effectively non-standard for human-facing HTTPS resources.

We propose a suite of HTTPS headers, HTML head elements, and surrounding utilities to make direct monetization of web-accessible resources low-stress and safe even for non-specialist human users.

By design, this standard covers use-cases in which a 402 response would never actually be sent. In particular, we anticipate a tip-jar or crowd-funding configuration in which no payment is _required_ to access the resource, but a clear means of making optional payments is exposed to the client along with explicit expectations about what they _should_ pay. Throughout this document we will talk about interchangeably about "requiring" or "accepting" receipts. The actual behavior gets specified in the Receipt Definition.

Although this standard will focus on HTTPS exchanges; it's desirable that the framework of translate clearly to other internet communications. 

## Context for a 402 response
It's generally poor user experience to reject a web-page request outright. A 402 response to such a request may work if a suitable placeholder page is available and the client is trusted to handle the response appropriately.

In practice, it is usually better to give  normal response to the principal request and require receipts for a key resource used by that page, for example an image or text-block. To avoid performance impacts, a page should [provide instructions]({{ "communicate.html" | relative_url }}) about what receipts the secondary resource will require. **This information must be accurate to avoid a visitor paying for a useless receipt.**

## Some early concerns:
- The duplication of HTTPS header specifications and in-line HTML feels weird, but is probably the way forward. In particular, in-line HTML is probably easier for most website authors to implement and HTTPS headers aren't accessible from JS, but HTTPS headers feel like they'll have better separation of concerns, work with images, and are probably friendlier for robots.
- Having the signatories do their signatures blindly is highly desirable, but fundamentally incompatible with passive-host design. 

