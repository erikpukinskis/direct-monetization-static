---
title: Receipts
date: 2018-12-06 12:00:00 Z
permalink: "/receipts.html"
description: The 402-Receipt Proposed standard.
documentation_order: 4
---

A receipt is a message specifying that someone paid for access to a resource, or should otherwise be given access to the resource.

A receipt is signed by an issuer; in principal anyone may issue and sign receipts, but the receiving party is only bound to honor receipts signed by the signatories specified in the Receipt Definition. It is up to the parties involved to agree in advance what is promised when a receipt is signed; here we assume that the signatory has collected the money in question from the requester and will forward those funds to the host by some outside channel.

A **Bare Receipt** is an object, represented in json, describing payment made for a particular item at a particular time. In the simple case a receipt will contain

- A unix timestamp `time`, as an integer representing seconds. Parties shall generally assume that the timestamp is accurate, even in the face of contradicting evidence. 
- A `unit` of currency. [ISO4217](https://en.wikipedia.org/wiki/ISO_4217) codes are recommended. 
- An amount `amount` of that currency, as a decimal number in the currency's major units.
- A namespace `domain`, typically a domain name, for the item being paid for.
- An `item` identifier, typically the uri of the resource.
- A `uuid` for the receipt itself. This may be used to prevent reuse of a receipt by multiple parties.

Two other formats of receipt are anticipated in this standard, covering cases where the requester should not be charged to access the resource.

- In the case that the user shouldn't have to pay for the resource because of some outside agreement, no `unit` or `amount` will be included. Instead a `plan` string specifying the agreement under which the signatory agrees the user should have access will be included.
- In the case that the user should have access because of payments they have made _for other resources_, a list `list` of the `uuid`s of those receipts will be included in place of the `unit` and `amount`. The host will typically enforce that the submission signature of such a receipt match the submission signatures of all the listed receipts. 

In order to be submitted, a Bare Receipt must be signed twice to form a **Signed Receipt** (commonly just called a **Receipt**).  
`TODO: define conventions for the siguature algorithm choice and communications. Blind signature would be nice for the 3rd party`  
First the client must find a 3rd-party signatory that will be accepted by the host and get a signature from them for the Bare Receipt. Typically this will be the point where the client actually pays the amount specified.  
Next the client creates a signature of the same Bare Receipt json using a private key. The private/public key pair identifies the user as unique from other users; they may reuse a key pair or not depending if they wish to be identifiable as the same user over time. `TODO: submission timestamp`

A **Signed Receipt** is:  

- The percent encoding of
    - The new-line ("\n") delimited concatenation of
        - The Bare Receipt,
        - The 3rd-party signature,
        - The client signature,
        - The public key used to verify the client signature.

`TODO: make provisions for compression`

