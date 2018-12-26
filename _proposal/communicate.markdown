---
title: Advertising required receipts
date: 2018-12-06 12:00:00 Z
permalink: "/communicate.html"
description: The 402-Receipt Proposed standard.
documentation_order: 2
---

The preferred way to let a client know that certain resources require receipts is to use a menu.xml document.  
**Inline markers for resources at time-of-use are strongly desirable, but we need to find a suitably small extension from existing standards/conventions to support them.**  
Finally, a response can include information about receipts that _should have been_ submitted for that resource, typically as part of a 402 response.

## Menu XML
A menu.xml is conceptually similar to a [sitemap.xml](https://www.sitemaps.org/protocol.html), but it lists _resources_ (possibly HTML pages, but more frequently images or snippets) instead of HTML _pages_.  
`TODO: Provide a formal XML namespace definition`  
A menu.xml will be structured like so:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<resourceset xmlns="TODO">
  <scope>https://www.example.com/</scope>
  <resource>
    <loc>https://www.example.com/path/image.png</loc>
    <verbs>
      <verb>GET</verb>
      <verb>HEAD</verb>
    </verbs>
    <definitions>...</definitions>
    <usedby>
      <url>https://www.example.com/page.html</url>
      ...
    </usedby>
  </resource>
  ...
</resourceset>
```

| Tag | Description |
|---------|-------------|
| `scope` | An absolute or relative address prefix or  an absolute address. Specifically, if the `scope` ends in a `/` then the scope of the menu is understood to be all addresses for which the `scope` is a prefix. The `scope` should not have a query-string or hash; even in the case of an absolute address all variations distinguished by query-strings or hashs are included in the one scope. |
| `resource` | A resource (identified by the `loc` and `verbs`) for which receipts are required. All resources within the scope, _and all resources used by pages within the scope_, for which receipts are required or accepted, must be listed. |
| `loc` | The absolute address of the resource for which a receipt is required in order to access. If there is not a query string in the `loc`, it's assumed that query strings will not identify a different resource or necessitate different receipts; they may still be used. Differentiating resources by hashes is not inteligable; hashes are not allowed in the `loc` |
| `verbs` | Optional; if ommitted it's assumed that the HTTP verb won't affect the resource or required receipts |
| `verb` | An HTTP verb (technically, a [method](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods)). |
| `definitions` | The [Receipt Definition]({{ "/receipt_definitions.html" | relative_url }}) for the specified resource. |
| `usedby` | A list of the `url`s of pages that use the resource. `TODO: Is this actually necessary? A defined concept of "uses" _is_ necessary in order to be clear about what needs to be in scope, but provided people are compling with the "in scope" requirements the n-n relationships wouldn't need to be listed. At a minimum, a wildcard system for the `url`s would be good.` |

While it is permissibly for a page to use multiple resources that require separate receipts, clients may perceive this as abuse and take unintended actions such as blocking the resources or prompting the user for action. Hosts are advised to use only a single receipt-requiring resource on each page, or to have all resources used by a page accept a common receipt.

In order for clients to know about a menu, pages should include an HTTPS header or an HTML meta tag pointing to a menu that includes that page in its scope.

```text
Receipts-Menu: https://www.example.com/menu.xml
```
```html
<meta property="receipts:menu" content="https://www.example.com/menu.xml" />
```

It's perfectly intelligible for each page to have its own menu; in this situation the host is encourage to use [data URIs](https://en.wikipedia.org/wiki/Data_URI_scheme) instead of serving the menu separately.

## Response headers
A resource that accepts receipts should say so in the response header (or, equivilantly, an HTML `meta` tag) even when responding to a request that already included the needed receipt. `TODO: give an explaination for how to compress a [Receipt Definition]({{ "/receipt_definitions.html" | relative_url }}) for use below.`

```text
Receipts-Accepts: [definitions]
```
```html
<meta property="receipts:accepts" content="[definitions]" />
```

A 200 response should not be given unless the receipts supplied with the request (including, possibly, no receipt at all) already fulfilled the requirements.

