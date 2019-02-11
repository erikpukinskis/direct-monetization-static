---
title: Receipts
date: 2018-12-06 12:00:00 Z
permalink: "/receipts.html"
description: The 402-Receipt Proposed standard.
documentation_order: 5
summary: How receipts are composed, signed, used, and validated. (work in progress)
---

A receipt is a message specifying that someone paid for access to a resource, or should otherwise be given access to the resource.

> #### The exact formatting, encoding, cryptographic signatures, and compression scheme for receipts have not yet been determined.
>
> In particular, a partially blind signature is desirable for the receipt; we hesitate to commit to other details until a suitable (portable, trusted) implementation can be identified.

There are several objects nested inside each other that, in different circumstances, might each be called a receipt.

- A **Receipt Submission** is the value of a `Receipts-Receipt` HTTP request header. It claims ownership of the contained Signed Receipt, and it claims that the Signed Receipt is sufficient to get access to the requested recourse at the time of the request.
- A **Signed Receipt** is issued by a Notary to mark that a Client has made the payments described in the Bare Receipt.
- A **Bare Receipt** describes a particular payment for a particular resource.

## Bare Receipt
A Bare Receipt is a serialized object describing payment made for a particular item at a particular time. A Bare Receipt will contain

- A unix timestamp `time`, as an integer representing seconds.
- A `unit` of currency. [ISO4217](https://en.wikipedia.org/wiki/ISO_4217) codes are recommended. 
- An amount `amount` of that currency, as a decimal number in the currency's major units.
- A name-space `domain`, typically a domain name, for the item being paid for.
- An `item` identifier, typically the URL path of the resource.
- A `uuid` for the receipt itself. This may be used to prevent reuse of a receipt by multiple parties.
- In the case that the user shouldn't have to pay for the resource because of some outside agreement, no `unit` or `amount` will be included. Instead a `plan` string specifying the agreement under which the notary determined that the user should have access will be included.

## Signed Receipt
The client will obtain a signature for the receipt from the chosen notary using whatever channel the notary provides (including, at minimum, the canonical URL). This combined receipt can be saved for repeated use within the TTL of the relevant Receipt Definitions.  
`TODO: We don't want it to be possible/convienent to share receipts. To an extent this is solved by the Client Signature and by giving receipts unique IDs. More problematic is the possibility that users might share the private keys used for the Client Signature. A complete solution is probably impossible or impractical, but a partial strategy to disincentivize sharing of accounts is needed. Note that _complete_ sharing of accounts among, for example, family members, may be acceptable; what needs to be prevented is one user sharing _receipts_ with another _at no cost to theirself_. This can maybe be done by making the private key used for the Client Signature _sufficient_ information to obtain Notary Signatures; i.e: if you give me the key I would need to use your receipts, then you've also given me what I need to transact funds out the account you presumably have with the Notary. Of course having the Host check the Client's public key with the Notary would compromise the blind signature, but we could include a Notary signature of the Client's public key in the Receipt Submission.`

## Receipt Submission
The receipt submission will contain the Signed Receipt (Bare Receipt plus Notary signature) and a time-of-use timestamp to be compared against the freshness requirement of the Receipt Definition. It also includes the Client's signature of the receipt/time-of-use pair, and the public key needed to check that signature. This public key is used by the Host as an identifier for the Client; the Client may reuse a key pair or not depending if they wish to be identifiable as the same user over time.

## Bundled Receipts
In the case that the Client should have access because of previously submitted payments they have made _for other resources_, a list of the `uuid`s of those receipts may be used in place of the Signed Receipt in the Receipt Submission. The Host will typically enforce that the Client signature of such a receipt match the submission signatures of all the listed receipts.  

For example, a website might give Receipt Definitions to the effect of _"Pages are \$0.05 each, up to \$3.00/month."_. Once a customer had viewed enough pages that their receipts for that month totaled \$3.00, their Client would begin submitting Bundled Receipts instead of buying a receipt for each page. 

