---
title: Parties
date: 2018-12-27 16:00:00 Z
permalink: "/parties.html"
description: Description of the roles of the Client, Host, and Notary.
documentation_order: 3
summary: Describes the roles of the Client, the Host, and the Notary. Also outlines
  the behavior of the tools they'll use.
---

Because the 402-Receipts system is mostly passive from the perspective of the human user, we mostly speak in terms of the Client (the web-browser).  
The Host is _not_ just "the website". The Host is the computer (the server) which provides the web pages to the Client.
The Notary is a third party, analogous to a traditoinal monetization platform but fundementally less powerful. The Notary sells Receitps to Clients; the Receipts are used to access websites.

## The Client
The Client is requesting a resource for which a receipt is required.  
It needs to detect references to [menu.xml files]({{ "/http.html" | relative_url }}) and parse them so that it can obtain receipts to include with reqests _on the first try_.
It needs to compose the [Bare Receipt]({{ "/receipts.html" | relative_url }}) and communicate with the [Notary]({{ "/notary.html" | relative_url }}) to get it signed.
Finally, it will include the Receipt in the Header of the HTTPS request.

The Client is an HTTPS client, typically a web browser being used by a human to view content. Existing web browsers don't have the behavior defined in this standard, so the user will need to install a browser plugin.  
Javascript included on pages served may also be viable, and are strongly desireabl as they may be easier for some users to use. We need a clear idea of how that can work, and how it can support the same privacy and trustworthiness guarantees, before we can promise that it will be an option.

A human user should be able to share receipts with them-self across devices. To do this effectively and securely with the signature scheme we intend will require a secure, encrypted, resiliant storage location for the user that's shared accross devices. We reffer to this as **the Wallet**. This could be left as an implementation detail, but if a javascript-only Client is possible then this standard will need to specify how it should communicate with a Wallet.

## The Host
The Host is the server receiving the HTTPS request with the `Receipts-Receipt` header. We assume that the Host's address will match the `domain` (and often `item`) of the Bare Receipt, and this may be enforced by the Client or Notary.

The Host's role is to validate the receipt; they must accept any receipt satisfying the Receipt Definition. If appropreit Receipts are provided, then the Host will behave like any other HTTPS host. If the Receipt is missing, inapropreate, or invalid the the Host will give an informative [402 Response]({{ "/http.html" | relative_url }}).

Assuming we stick with a blind-signature scheme for [Receipts]({{ "/receipts.html" | relative_url }}), a truly static Host will not be able to use this standard. At absolute minimum the host will need to be able to strip the identifying key and signature and forward the receipt and Notary signature to the Notary; otherwise the Notary would never know to pay the Host.

In practice, the Host should also be obliged to validate the receipt as part of request handling, and the Host **should not** forward the receipt until handling of the HTTPS request in question has finished. Submitting receipts to the Notary in batches (for example, every hour) can (in conjunction with some defensive behavior on the part of the Clients) prevent deanonymization of Receipts by timing analysis. 

Furthermore, the Host will need to maintain a database of receipts it has received which would still be valid for use along with the Client Key they were originally submitted with. This is needed to prevent sharing of receipts between consumers.

## The Notary
The Notary signs the receipt. It is up to the parties involved to agree in advance what is promised when a receipt is signed; here we assume that the Notary has collected the money in question from the Client and will forward those funds to the Host by some outside channel. In this sense the "Notary" is properly thought of as a "Broker". Of course it's the Notary's responsibility to make themselves valuable to the other parties, but it's illuminating to explain here some of the ways they might do that.

- The Notary will generally relieve the Host of payment processing complexity, and account-management more generally.
- When a Host joins a Notary's network, they hope to receive payments from a wide audience of consumers; the Host can focus on _traffic_ and _audience retention_ instead of a _sales funnel_. 
- The Notary can protect the Client from abusive Hosts in various ways, including proactive monitoring of member Hosts and post-hoc remediation. (Point-of-sale protections are generally not feasible for the Notary, as receipt signatures are issued blindly. Such protections can be built into the [Wallet]({{ "client.html" | relative_url }}).)
- For a Client to set up an account with a given Notary generally constitutes pre-authorization of certain kinds of payments for certain kinds of resources. These kinds of restrictions and protections can be configured with the Notary or in the Wallet.

In practice, we only anticipate two kinds of Notaries.

- A Host could be their own Notary. This could accomplish any of the same functionalities as existing paywalls offer, although the standardized machine interface would enable smoother user-experiences. 
- **A Notary can act as a network, channel, buyer's club, or federation.** Many Hosts and many Clients are assumed to have prior arrangements (accounts) with a given Notary; this is the Notary's network. As long as a Host and Client have a network in common, payment for resources can typically proceed _automatically_.

Notaries with larger networks will obviously have an advantage.  
On the one hand, the variety of use-cases this standard can accommodate, and the variety of legal, technical, and business paradigms people operate in, will likely ensure a plurality of Notaries. On the other hand, it is still desirable to include protections _against the Notaries_. Consumers are presumed to have given their payment processing information (a debit card, for example) directly to the Notary, but privacy of their data can still be protected by insisting on a blind-signature scheme, and by proper design of Wallets and Clients. Producers (Hosts) can best be protected by making sure self-Notarization is a viable option (although this does put the onus of payment processing back in their lap).

{% include glossary_item s=site.data.glossary.cannonical_url %}


