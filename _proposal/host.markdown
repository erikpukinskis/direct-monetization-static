---
title: The Host
date: 2018-12-27 16:00:00 Z
permalink: "/host.html"
description: The host's role
documentation_order: 40
---

The **Host** is the server receiving the HTTP request with the `Receipts-Receipt` header. We assume that the Host's address will match the `domain` (and often `item`) of the Bare Receipt, and this may be enforced by the Client or Notary. The Host's role is to validate the receipt; they must accept any receipt satisfying the Receipt Definition.
