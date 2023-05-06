---
title: Integrate Traefik with getlocalcert.net
summary: How to integrate Traefik with getlocalcert.net
---

# Traefik

[Traefik](https://traefik.io/traefik/) is a cloud native application proxy.

!!! warning "Untested"

    This page needs review

## Issue a certificate

``` yaml
certificatesResolvers:
  myresolver:
    acme:
      dnsChallenge:
        provider: acme-dns
```


See also:

* https://doc.traefik.io/traefik/https/acme/#dnschallenge

