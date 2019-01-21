---
title: Overview
date: 2018-12-06 12:00:00 Z
permalink: "/overview.html"
description: Overview of the 402-Receipt Proposed standard.
documentation_order: 2
summary: An overall summary of how the system works.
---

We propose a suite of HTTPS headers, HTML head elements, and surrounding utilities to make direct monetization of web-accessible resources low-stress and safe even for non-specialist human users.

By design, this standard covers use-cases in which a 402 response would never actually be sent. In particular, we anticipate a tip-jar configuration in which no payment is _required_ to access the resource, but a clear means of making optional payments is exposed to the client along with explicit expectations about what they _should_ pay. Throughout this document we will talk interchangeably about "requiring" or "accepting" receipts. The actual behavior gets specified in the Receipt Definition.

Although this standard will focus on HTTPS exchanges; it's desirable that the framework translate clearly to other internet communications. 

## How do you, an end-user, use this?

In order to participate, you'll need to create an account with at least one [Notary]({{ "/notary.html" | relative_url }}). You'll need your web-browser to be logged into some sort of wallet; this will probably be a browser plugin, but we want to have javascript-cookie based options too. The wallet is the only part of the system that both knows who you are _and_ what you're doing.  
If you aren't logged in properly, you may see prompts or placeholders as you browse the web.

As you browse the web, websites you visit will tell your web-browser how much their pages cost. They do this using Header tags in their web-pages that direct your browser to a menu of pages you _might_ visit; this happens in the background without you needing to do anything.

When you navitage to a page that costs money, if your wallet is configured to allow the transaction (based on the amount and possibly details about the website itself), then it will get a receipt for the needed amount from the Notary. This receipt is passed to the website, which then gives you access to the page or content in question. This happens in the background without you needing to do anything; as a human user you only need to worry about the payments system when you're configuring your account or if you don't like what your Client is doing.

  
### Context for a 402 response
It's generally poor user experience to reject a web-page request outright. A 402 response to such a request may work if a suitable placeholder page is available and the client is trusted to handle the response appropriately.

In practice, it is usually better to give  normal response to the principal request and require receipts for a key resource used by that page, for example an image or text-block. To avoid performance impacts, a page should [provide instructions]({{ "communicate.html" | relative_url }}) about what receipts the secondary resource will require. **This information must be accurate to avoid a visitor paying for a useless receipt.**

### Consumer tools:
We anticipate a distinction between the [Client]({{ "/client.html" | relative_url }}) (typically a web-browser, possibly with a plugin) and a remote [Wallet]({{ "/client.html" | relative_url }}) which will allow persistence of user-data between clients and devices.

### Producer tools:
Different [Hosts]({{ "/host.html" | relative_url }}) will have different needs. The textbook usage will be a server plugin, designated servlet, or specialized CDN, that will inspect requests for receipts, validate the receipts, and forward them to their respective Notaries for disbursement. 

### Brokers and Networks:
Throughout this standard we speak of a [Notary]({{ "/notary.html" | relative_url }}); the Notary's role is not just to sign receipts; they also handle the collection of money from consumers and disbursement of it to producers. In the typical case both the host and the client will have a prior relationship (membership or account) set up with one or more Notary; as long as they have one in common the user experience can proceed seamlessly.

## Some early concerns:
- The duplication of HTTPS header specifications and in-line HTML feels weird, but is probably the way forward. In particular, in-line HTML is probably easier for most website authors to implement and HTTPS headers aren't accessible from JS, but HTTPS headers feel like they'll have better separation of concerns, work with images, and are probably friendlier for robots.
- Having the signatories do their signatures blindly is highly desirable, but fundamentally incompatible with passive-host design. 

