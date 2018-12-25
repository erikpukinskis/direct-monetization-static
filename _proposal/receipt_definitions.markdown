---
title: Receipt Definitions
date: 2018-12-06 12:00:00 Z
permalink: "/receipt_definitions.html"
description: The 402-Receipt Proposed standard.
documentation_order: 5
---

A **Receipt Definition** must make clear to a client what receipts would be accepted in order to access the relevant resource. In particular it contains

- requirements for the receipt contents
- specification about what 3rd-party signatories would be acceptable.

Figuring out what the client will need to do in order for the chosen signatory to actually sign the receipt must be handled out-of-band; it's assumed that either the client has prior contact with the signatory or human-user action will be required to complete the payment. `TODO: maybe give an in-band fall-back method?`

A **Receipt Definition** is an XML object within the same namespace as a Menu XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<definitions xmlns="TODO">
  <definition>
    <domain>https://www.example.com/</domain>
    <item>/path/image.png</item>
    <signers>
      <signer>https://receipts.dmn.network/path</signer>
      <signer>https://www.alt.com/path</signer>
    </signers>
    <ttl>2720000</ttl>
    <fresh>60</fresh>
    <costs>
      <cost>
        <units>USD</units>
        <amount>0.05</amount>
      <cost>
      <cost>
        <units>XRP</units>
        <amount>0.15</amount>
      <cost>
    </costs>
  </definition>
  <definition>
    <domain>https://www.example.com/</domain>
    <item></item>
    <signers>
      <signer>https://receipts.dmn.network/path</signer>
      <signer>https://www.alt.com/path</signer>
    </signers>
    <ttl>2720000</ttl>
    <fresh>60</fresh>
    <costs>
      <cost>
        <units>USD</units>
        <amount>2.0</amount>
      <cost>
      <cost>
        <units>XRP</units>
        <amount>6.0</amount>
      <cost>
    </costs>
  </definition>
  <definition>
    <domain>https://www.example.com/</domain>
    <item></item>
    <signers>
      <signer>https://receipts.dmn.network/path</signer>
    </signers>
    <ttl>2720000</ttl>
    <fresh>60</fresh>
    <costs>
      <cost><plan>dmn-subscriber</plan></cost>
    </costs>
  </definition>
  <definition><none></none></definition>
</definitions>
```

| Tag | Description |
|---------|-------------|
| `definition` | To access the resource, a receipt matching one of these descriptions is needed. A `definition` tag will either contain a single `none` tag, or all the other child tags. |
| `none` | A `<definition><none></none></definition>` element indicates that while any of the other defined receipts would be _accepted_, no receipt is _required_. `none` has no children. |
| `domain` | For an HTTPS resource, then this _must_ match the domain of the resource's address. |
| `item` | This will typically be the path of the HTTP resource, but it could be anything. An _empty_ `item` tag indicates that any list of receipts matching the rest of the definition and totaling to the cost will suffice. |
| `signer` | An absolute HTTPS url identifing a signatory. `TODO: The exact use of these urls will be pinned down as we figure out the details of the signing process.` Any one of the listed signatories is sufficient. |
| `ttl` | An integer representing the maximum age in seconds of a Signed Receipt that will be accepted. Two independent conventions are recommended as "normal": The `ttl` should be 2720000, comfortably covering a month. The `ttl` should be the same for all items in a given domain. |
| `fresh` | An integer representing the maximum age in seconds of a Receipt Submission. For now this is required to be 60. |
| `cost` | If multiple costs are listed in a single definition, they should probably represent similar monitary values; `costs` is a list to accomidate multiple currencies. A `cost` tag will have either a `plan` child or a `units` and an `amount` child
| `units` | A currency code. [ISO4217](https://en.wikipedia.org/wiki/ISO_4217) codes are recommended. |
| `amount` | A decimal number amount of that currency's major units. |
| `plan` | An identifier which is assumed to mean something to the various parties involved.

`TODO: This should probably get compressed, and maybe more explanaintion about how to read it and why it's shaped like it is would be good.`

