---
title: Advertising required receipts
date: 2018-12-06 12:00:00 Z
permalink: "/communicate.html"
description: The 402-Receipt Proposed standard.
documentation_order: 2
---

The preferred way to let a client know that certain resources require receipts is to use a menu XML document.  
**Inline markers for resources at time-of-use are strongly desirable, but we need to find a suitably small extension from existing standards/conventions to support them.**  
Finally, a response can include information about receipts that _should have been_ submitted for that resource.

## Menu XML
A menu XML is conceptually similar to a [sitemap.xml](https://www.sitemaps.org/protocol.html).  
`TODO: Provide a formal namespace definition`

A menu XML will be structured like so:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<resourceset xmlns="TODO">
  <scope>
    https://www.example.com/
  </scope>
  <resource>
    <loc>https://www.example.com/path/image.png</loc>
    <acceptreceipts>Receipt Definition</acceptreceipts>
    <usedby>
      <url>https://www.example.com/page.html</url>
      ...
    </usedby>
  </resource>
  ...
</resourceset>
```

Clients will assume that a menu XML is exhaustive, in the sense that all resources for which the menu's scope is a prefix are either included in the menu or do not require receipts, and no page for which the menu's scope is a prefix will us a resource that requires receipts except as specified in the menu.

While it is permissibly for a page to use multiple resources that require separate receipts, clients may perceive this as abuse and take unintended actions such as blocking the resources or prompting the user for action. Hosts are advised to use only a single receipt-requiring resource on each page, or to have all resources used by a page accept a common receipt.

In order for clients to know about a menu, pages should include a meta tag in their head pointing to a menu that includes that page in its scope.

```html
<meta property="receipts:menu" content="https://www.example.com/menu.xml" />
```
  
An HTTP header can also be used, and may be preferable depending on the implementation.

```text
Receipts-Menu: https://www.example.com/menu.xml
```

It's perfectly intelligible for each page to have its own menu; in this situation the host is encourage to use [data URIs](https://en.wikipedia.org/wiki/Data_URI_scheme) instead of serving the menu separately.

## Response headers
A resource that accepts receipts should say so in the response header, even when responding to a request that already included the needed receipt.

```text
Receipts-Accepts: Receipt Definition
```

An `<meta>` tag can represent the same information, but may require more careful consideration.

```html
<meta property="receipts:accepts" content="Receipt Definition" />
```

Such a tag included in the HTML payload of a 402 response would indicate that a different (more complete) version of the page would have been given if the request had included a receipt; this is fine but not an available option for non-html resources.  
In a 200 HTML payload it would indicate the supplied receipts (including, possibly, no receipt at all) already fulfilled the requirements. This is less suitable for a tip-jar configuration than it may seem: The client shouldn't have to repeat a request to submit an optional receipt.

