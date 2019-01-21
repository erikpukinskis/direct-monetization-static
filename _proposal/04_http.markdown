---
title: Advertising required receipts
date: 2018-12-06 12:00:00 Z
permalink: "/http.html"
description: The 402-Receipt Proposed standard.
documentation_order: 2
---

Most of the communication needed by this standard happens in the HTTP headers of requests and responses.  
For ease of implementation, some items may be specified in HTML. Specifically, the address of a menu.xml may be given in a `<meta>` tag instead of a Header. **Inline HTML Receipt Definitions are desireable, but have not yet been specified.**

Additionally, the meaning of relevant HTTP Response codes is explained.

**All requests and responses involved in this standard should be made of HTTPS connections with valid certificates.** Implementations are not obliged to accept receipts or other information transmitted over HTTP.

{% include glossary_item s=site.data.glossary.menu_header %}
{% include glossary_item s=site.data.glossary.accepts_header %}
{% include glossary_item s=site.data.glossary.receipt_header %}
{% include glossary_item s=site.data.glossary.402_response %}

