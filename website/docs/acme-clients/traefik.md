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

