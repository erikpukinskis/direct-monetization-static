---
title: The Host
date: 2018-12-27 16:00:00 Z
permalink: "/host.html"
description: The host's role
documentation_order: 40
---

The **Host** is the server receiving the HTTP request with the `Receipts-Receipt` header. We assume that the Host's address will match the `domain` (and often `item`) of the Bare Receipt, and this may be enforced by the Client or Notary. The Host's role is to validate the receipt; they must accept any receipt satisfying the Receipt Definition.

Assuming we stick with a blind-signature scheme for [Receipts]({{ "/receipts.html" | relative_url }}), a truly static Host will not be able to use this standard. At absolute minimum the host will need to be able to strip the identifying key and signature and forward the receipt and Notary signature to the Notary; otherwise the Notary would never know to pay the Host.

In practice, the Host should also be obliged to validate the receipt as part of request handling, and the Host **should not** forward the receipt until handling of the HTTPS request in question has finished. Submitting receipts to the Notary in batches (for example, every hour) can (in conjunction with some defensive behavior on the part of the Clients) prevent deanonymization of receipts by timing analysis. 

Furthermore, the Host will need to maintain a database of receipts it has received which would still be valid for use along with the Client Key they were originally submitted with. This is needed to prevent sharing of receipts between consumers. 

