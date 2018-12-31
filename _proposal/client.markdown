---
title: The Client
date: 2018-12-27 16:00:00 Z
permalink: "/client.html"
description: The client's role
documentation_order: 20
---

The **Client** is requesting a resource for which a receipt is required.  
It needs to compose the [Bare Receipt]({{ "/receipts.html" | relative_url }}) and communicate with the [Notary]({{ "/notary.html" | relative_url }}) to get it signed.

Loosely speaking, the Client will always be an HTTPS client; in general, a web browser being used by a human to view content. Existing web browsers don't have the behavior defined in this standard; browser plugins are an ideal intermediary. Javascript included on pages served may also be viable, although they will not be able to support the same privacy and trustworthiness guarantees.

A human user should be able to share receipts with them-self across devices. To do this effectively and securely with the signature scheme we intend will require that the shared repository (**the Wallet**) be more that a static read-write system; the private keys should probably live in the Wallet and signing should happen there. This could be left as an implementation detail, but if a javascript-only option is possible then its operation should be specified here because javascript published by the [Host]({{ "/host.html" | relative_url }}) will need to communicate directly with the Wallet.


