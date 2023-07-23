---
title: Let's Encrypt
summary: How to set up Let's Encrypt with getlocalcert.net
---

# Let's Encrypt Setup

[Let's Encrypt](https://letsencrypt.org/) offers free TLS certificate and most ACME client work with Let's Encrypt without any special configuration.

Unfortunately, Let's Encrypt has strict [rate limits](https://letsencrypt.org/docs/rate-limits/).
You can usually avoid these by trying to issue certificates in the [staging environment](https://letsencrypt.org/docs/staging-environment/) first.

Consider [ZeroSSL](../zerossl/) if you are a large organization or require additional services.

