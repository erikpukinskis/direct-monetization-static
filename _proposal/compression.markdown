---
title: Compression
date: 2018-12-31 15:44:00 Z
permalink: "/compression.html"
description: Conventions for compressing objects within the context of the 402-Receipts standard.
documentation_order: 100
---

There are a few places throughout the standard, particularly in HTTPS Headers, where compression is required.  
Usually this is to reduce the likelyhood of problems with HTTPS Header size limits. The parties are strongly recommended to check the size of the compressed string against a reasonable limit (10KB for an HTTPS Header); if no remediation for an over-sized message is avaliable it may be better to throw a runtime exception than to send messages that can't be received. 

The data, as UTF-8 bytes, should be compressed in gzip format and then rendered as base64.

- Is the above description sufficiently specific? Is "gzip" the correct term for the (language-nonspecific) function to use?
- Should this compression be optional? After all, even an uncompressed [Receipt Definition]({{ "/receipt_definitions.html" | relative_url }}) will typically be less than 10KB. 
- If we really want to be parsimonious about bytes, the data objects themselves could be designed to be denser, or we could render the compressed message in base85. Probably neither of these are good ideas.

