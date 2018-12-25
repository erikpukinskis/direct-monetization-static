---
title: The 402 Response Code
date: 2018-12-06 12:00:00 Z
permalink: "/response.html"
description: The 402-Receipt Proposed standard.
documentation_order: 3
---

A 402 HTTP response indicates that the request should/may be retried with a receipt as described in the `Accept-Receipts` header. To the extent possible, 402 should be the "rejection of last resort" in the sense that a 402 response should not be given if the request would have failed for other reasons in addition to the absence of a suitable `Receipt` header.

If a 402 response has a response body, that body should be used as a placeholder for the requested resource, and must be appropriate for such use. If a 402 response has a `Link: <url>; rel=alternate` header, then it must specify a placeholder for the requested resource and there should not be a body.

