# The 402-Receipt Proposed standard
#### Version negative one.

The 402 response code implies that a request is being denied because payment must be provided for the requested resource. No standard way of following up to a 402 response has been normalized, as the 402 response code has seen only neiche useage, mostly for subscription-only APIs, and is effectivly non-standard for human-facing http resources.

We propose a suite of http headers, html head elements, and surrounding utilities to make direct monetization of web-accessable resources low-stress and safe even for non-specialist human users.

By design, this standard covers use-cases in which a 402 response would never actually be sent. We can think of this like a tip-jar or crowd-funding tool; no payment is _required_ to access the resouce, but a clear means of making optional payments is exposed to the client along with explicit expectations about what they _should_ pay.

Throughout this document we will talk about interchangeably about "requiring" or "accepting" receipts. The actual behavior gets specified in the Receipt Definition.

Although this standandard is only applicable for HTTP exchanges; it's desirable that the framework of receipts translate clearly to other internet communications. 

## Context for a 402 response
It's generally poor user experience to reject a web-page request outright. A 402 response to such a request may work if a suitable placeholder page is avaliable and the client is trusted to handle the response appropreatly.

In practice, it is usually better to give normal responses to the principal request, and require reciepts for a key resource used by that page, for example an image or text-block. To avoid performance impacts, a page should provide instructions about what receipts the secondary resource will require. **This information must be accurate to avoid a visitor paying for a useless receipt.**

## Some early concerns:
- The duplication of HTTP header specificaitons and in-line html feels weird, but is probably the way forward. In particular, in-line html is probably easier for a most website authors to implement and HTTP headers aren't accessable from JS, but HTTP headers feel like they'll have better separation of concerns, work with images, and are probably friendler for robots.
- Having the signatories do their signatures blindly is highly desirable, but fundementally incompatible with passive-host design. 

# Advertising required receipts
The prefered way to let a client know that certain resoures require receipts is to use a menu XML document.  
**Inline markers for resources at time-of-use are strongly desireable, but we need to find a suitablely small extension from existing standards/conventions to support them.**  
Finally, a response can include information about receipts that _should have been_ submitted for that resource.

## Menu XML
A menu XML is conceptually similar to a [sitemap.xml](https://www.sitemaps.org/protocol.html).  
`TODO: Provide a formal namespace definition`

A menu XML will be structured like so:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<resourceset xmlns="TODO">
  <scope>
    https://www.example.com/
  </scope>
  <resource>
    <loc>https://www.example.com/path/image.png</loc>
    <acceptreceipts>Receipt Definition</acceptreceipts>
    <usedby>
      <url>https://www.example.com/page.html</url>
      ...
    </usedby>
  </resource>
  ...
</resourceset>
```

Clients will assume that a menu XML is exhaustive, in the sense that all resources for which the menu's scope is a prefix are either included in the menu or do not require receipts, and no page for which the menu's scope is a prefix will us a resource that requires receipts except as specified in the menu.

While it is permissably for a page to use multiple resources that require seperate reciepts, clients may percive this as abuse and take unintended actions such as blocking the resouces or prompting the user for action. Hosts are advised to use only a single receipt-requiring resource on each page, or to have all resources used by a page accept a common receipt.

In order for clients to know about a menu, pages should include a meta tag in their head pointing to a menu that includes that page in its scope.

```html
<meta property="receipts:menu" content="https://www.example.com/menu.xml" />
```
  
An HTTP header can also be used, and may be preferable depending on the implementation.

```text
Receipts-Menu: https://www.example.com/menu.xml
```

It's perfectly inteligable for each page to have its own menu; in this situation the host is encourage to use [data URIs](https://en.wikipedia.org/wiki/Data_URI_scheme) instead of serving the menu seperatly.

## Response headers
A resource that accepts receipts should say so in the response header, even when responding to a request that already included the needed receipt.

```text
Receipts-Accepts: Receipt Definition
```

An `<meta>` tag can represent the same information, but may require more careful consideration.

```html
<meta property="receipts:accepts" content="Receipt Definition" />
```

Such a tag included in the HTML payload of a 402 response would indicate that a different (more complete) version of the page would have been given if the request had included a receipt; this is fine but not an avaliable option for non-html resources.  
In a 200 HTML payload it would indicate the supplied receipts (including, possibly, no receipt at all) already fulfilled the requirements. This is less suitable for a tip-jar configuration than it may seem: The client shouldn't have to repeat a request to submit an optional receipt.

## The Receipt Definition
`TODO: define the Receipt Definition`

# Receipts
A receipt is a message specifying that someone paid for access to a resource, or should otherwise be given access to the resource.

A receipt is signed by an issuer; in principal anyone may issue and sign receipts, but the recieving party is only bound to honor receipts signed by the signatories specified in the Receipt Definition. It is up to the parties involved to agree in advance what is promised when a receipt is signed; here we assume that the signatory has collected the money in question from the requestor and will forward those funds to the host by some outside channel.

A **Bare Receipt** is an object, represented in json, describing payment made for a particular item at a particular time. In the simple case a receipt will contain

- A unix timestamp `time`, as an integer representing seconds. Parties shall generally assume that the timestamp is accurate, even in the face of contradicting evidence. 
- A `unit` of currency. [ISO4217](https://en.wikipedia.org/wiki/ISO_4217) codes are recomended. 
- An amount `amount` of that currency, as a decimal number in the currency's major units.
- A namespace `domain`, typically a domain name, for the item being paid for.
- An `item` identifier, typically the uri of the resource.
- A `uuid` for the receipt itself. This may be used to prevent reuse of a receipt by multiple parties.

Two other formats of receipt are anticipated in this standard, covering cases where the requestor should not be charged to access the resource.

- In the case that the user shouldn't have to pay for the resource because of some outside agreement, no `unit` or `amount` will be included. Instead a `plan` string specifying the agreement under which the signatory agrees the user should have access will be included.
- In the case that the user should have access because of payments they have made _for other resources_, a list `list` of the `uuid`s of those receipts will be included in place of the `unit` and `amount`. The host will typically enforce that the submission signature of such a receipt match the submission signatures of all the listed receipts. 

In order to be submitted, a Bare Receipt must be signed twice to form a **Signed Receipt** (commonly just called a **Receipt**).  
`TODO: define conventions for the siguature algorithm choice and communications. Blind signature would be nice for the 3rd party`  
First the client must find a 3rd-party signatory that will be accepted by the host and get a signature from them for the Bare Receipt. Typically this will be the point where the client actually pays the amount specifed.  
Next the client creates a signature of the same Bare Receipt json using a private key. The private/public key pair identifies the user as unique from other users; they may reuse a key pair or not depending if they wish to be identifiable as the same user over time. `TODO: submission timestamp`

A **Signed Receipt** is:  

- The precent encoding of
    - The new-line ("\n") delimited concatination of
        - The Bare Receipt,
        - The 3rd-party signature,
        - The client signature,
        - The public key used to verify the client signature.

`TODO: make provisions for compression`

# Receipt Definitions
A **Receipt Definition** must make clear to a client what it will need to do to make a receipt that will be accepted in order to access the relevant resource. In particular it contains

- requirements for the receipt contents
- specificaiton about what 3rd-party signatories would be acceptable.

Figuring out what the client will need to do in order for the chosen signatory to actually sign the receipt must be handled out-of-band; it's assumed that either the client has prior contact with the signatory or human-user action will be required to complete the payment. `TODO: maybe give an in-band fall-back method?`

A **Receipt Definition** is an object represented in json:

- Optionally, a `domain`. If provided in the Receipt Definition for an HTTP resource, then this _must_ match the domain of the resource's address.
- Optionally, an `item` identifier. If not provided, then any list of receipts matching the other specificaitons and totaling to the needed amount will suffice.
- A list of objects of the form
    - `signers`: A list of acceptable signatories, identified by urls.
    - `ttl`: An integer representing the maximum age in seconds of a receipt that will be accepted. The following two independent convetions are strongly recommended as "normal": The `ttl` should be 2720000, comfortably covering a month. The `ttl` should be the same for all items in a given domain.
    - `fresh`: An integer representing the maximum age in seconds of a receipt submission. For now, this is required to be 60.
    - `cost`: A list of objects each containging
        - `units`: A currency code.
        - `amount`: A decimal number amount of that currency's major units.

`TODO: This should probably get compressed, and maybe more explanaintion about how to read it and why it's shaped like it is would be good.`

# The 402 Response Code
A 402 response indicates that the request should/may be retried with a reciept as described in the `Accept-Receipts` header. To the extent possible, 402 should be the "rejection of last resort", in the sense that a 402 response should not be given if the request would have failed for other reasons in addition to the absence of a suitable `Receipt` header.

If a 402 response has a response body, that body should be used as a placeholder for the requested resource, and must be appropreate for such use. If a 402 response has a `Link: <url>; rel=alternate` header, then it must specify a placeholder for the requested resource. `is that a good idea?`


--------
--------


##### Junk
- Request headers
- Server behavior
- Client behavior
- Javascript semi-independent client
- Wallet daemon
- Receipt issuer
- Server plugin(s)
- Client plugin(s)
- Reciept issuer(s)
- Cloud wallet
- JS widgit