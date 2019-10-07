---
layout: post
title:  "Generate Public/Private keys"
date:   2019-04-30 01:30:13 +0000
categories: Tools
tags: SSL Keys
---

In order to push data on the platform, you need to share its public key with the
server. This public key enables the server to authenticate messages coming from
your thing. For this we use the tool openssl.

In the terminal, type in the following command to generate a private key. The private
key is saved saved in private.pem.

```bash
openssl genrsa -out private.pem 4096
```

Then, extract the attached public key with the following command. The public key
is saved in public.pem

```bash
openssl rsa -in private.pem -outform PEM -pubout -out public.pem
```

