---
title: Receipt Definitions
date: 2018-12-06 12:00:00 Z
permalink: "/receipt_definitions.html"
description: The 402-Receipt Proposed standard.
documentation_order: 5
---

A **Receipt Definition** must make clear to a client what it will need to do to make a receipt that will be accepted in order to access the relevant resource. In particular it contains

- requirements for the receipt contents
- specification about what 3rd-party signatories would be acceptable.

Figuring out what the client will need to do in order for the chosen signatory to actually sign the receipt must be handled out-of-band; it's assumed that either the client has prior contact with the signatory or human-user action will be required to complete the payment. `TODO: maybe give an in-band fall-back method?`

A **Receipt Definition** is an object represented in json:

- Optionally, a `domain`. If provided in the Receipt Definition for an HTTP resource, then this _must_ match the domain of the resource's address.
- Optionally, an `item` identifier. If not provided, then any list of receipts matching the other specifications and totaling to the needed amount will suffice.
- A list of objects of the form
    - `signers`: A list of acceptable signatories, identified by urls.
    - `ttl`: An integer representing the maximum age in seconds of a receipt that will be accepted. The following two independent conventions are strongly recommended as "normal": The `ttl` should be 2720000, comfortably covering a month. The `ttl` should be the same for all items in a given domain.
    - `fresh`: An integer representing the maximum age in seconds of a receipt submission. For now, this is required to be 60.
    - `cost`: A list of objects each containing
        - `units`: A currency code.
        - `amount`: A decimal number amount of that currency's major units.

`TODO: This should probably get compressed, and maybe more explanaintion about how to read it and why it's shaped like it is would be good.`

