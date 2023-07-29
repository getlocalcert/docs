---
title: Let's Encrypt
summary: How to set up Let's Encrypt with getlocalcert.net
---

# Let's Encrypt Setup

[Let's Encrypt](https://letsencrypt.org/) offers free TLS certificate and most ACME client work with Let's Encrypt without any special configuration.

Unfortunately, Let's Encrypt has strict [rate limits](https://letsencrypt.org/docs/rate-limits/).
You can usually avoid these by trying to issue certificates in the [staging environment](https://letsencrypt.org/docs/staging-environment/) first.

Consider [ZeroSSL](../zerossl/) if you are a large organization or require additional services.

## Endpoints

When using Let's Encrypt you should first try issuing a certificate with the Staging environment.
This avoids getting rate limited.
Certificates created by the staging environment are not trusted by default on any browser, so you'll still see certificate warning messages, these are expected.

    https://acme-staging-v02.api.letsencrypt.org/directory

Once you've got that working, switch to Let's Encrypt Production which uses certificates that are widely trusted.

    https://acme-v02.api.letsencrypt.org/directory

