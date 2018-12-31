---
title: The Notary
date: 2018-12-27 16:00:00 Z
permalink: "/notary.html"
description: The notary's role
documentation_order: 30
---

The **Notary** signs the receipt. It is up to the parties involved to agree in advance what is promised when a receipt is signed; here we assume that the Notary has collected the money in question from the Client and will forward those funds to the Host by some outside channel. In this sense the "Notary" is properly thought of as a "Broker". Of course it's the Notary's responsability to make themselves valuable to the other parties, but it's illuminating to explain here some of the ways they might do that.

- The Notary will generally relieve the Host of payment processing complexity, and account-management more generally.
- When a Host joins a Notary's network, they hope to recieve payments from a wide audience of consumers; the Host can focus on _traffic_ and _audience retention_ instead of a _sales funnel_. 
- The Notary can protect the Client from abusive Hosts in various ways, including proactive monitoring of member Hosts and post-hoc remediation. (Point-of-sale protections are generally not feasable for the Notary, as receipt signatures are issued blindly. Such protections can be built into the [Wallet]({{ "client.html" | relative_url }}).)
- For a Client to set up an account with a given Notary generally constitues authorization of certain kinds of payments for certain kinds of resources. These kinds of restrictions and protections can be configured with the Notary or in the Wallet.

In practice, we only anticipate two kinds of Notaries.

- A Host could be their own Notary. This could accomplish any of the same functionalities as existing paywalls offer, although the standardized machine interface would enable smoother user-experiences. 
- **A Notary can act as a network, channel, buyer's club, or federation.** Many Hosts and many Clients are assumed to have prior arrangemnts (accounts) with a given Notary; this is the Notary's network. As long as a Host and Client have a network in common, payment for resources can typically proceed _automatically_.

There is an obvious advantage to larger networks. On the one hand, the variety of use-cases this standard can accomidate, and the variety of legal, technical, and business paradigms people operate in, will likely ensure a plurality of Notaries. On the other hand, it is still desireable to include protections _against the Notaries_. Consumers are presumed to have given their payment processing informaiton (a debit card, for example) directly to the Notary, but privacy of their data can still be protected by insisting on a blind-signature scheme, and by proper design of Wallets and Clients. Producers (Hosts) can best be protected by making sure self-Notarization is a viable option (although this does put the onus of payment processing back in their lap).

A Notary is required to expose a canonical URL endpoint by which they can be identified, and which is _sufficient_ to interact with that Notary.

- A GET request to the canonical URL should return an HTML page which a human could use to complete a purchase and obtain a receipt. In practice this page would probably also prompt the user to open an account with the Notary. `TODO: specify how a receipt could be passed into this page from the URL hash for blind-signing and how the Notary's signature would be communicated back to the Client.`
- A POST request to the canonical URL is used to obtain a Notary Signature; a signature with a pre-approved Client's private key should be sufficient authentication for this. `TODO: the details of this process will be filled in as the cryptographic detials of receipt signing are determined.`
- A PUT request to the canonical URL is used by the HOST to redeem receipts, in batches.

