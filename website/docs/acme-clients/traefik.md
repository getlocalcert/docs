---
title: Traefik
summary: How to integrate Traefik with getlocalcert.net
---

# Traefik

[Traefik](https://traefik.io/traefik/) is a cloud native application proxy.

## API Keys

If you haven't already, setup an API key for your subdomain in [the console](https://console.getlocalcert.net/).
Save your subdomain information and credentials to a JSON file like this:

``` json title="credentials.json"
{
  "username": "<yourApiKeyId>",
  "password": "<yourApiKeySecret>",
  "fulldomain": "<yourSubdomain>.localhostcert.net",
  "subdomain": "<yourSubdomain>",
  "server_url": "https://api.getlocalcert.net/api/v1/acme-dns-compat",
  "allowfrom": []
}
```

LEGO uses a slightly different format to manage these keys:

``` json title="lego-creds.json"
{
  "<yourSubdomain>.localhostcert.net": {
    "username": "<yourApiKeyId>",
    "password": "<yourApiKeySecret>",
    "fulldomain": "<yourSubdomain>.localhostcert.net",
    "subdomain": "<yourSubdomain>",
    "server_url": "https://api.getlocalcert.net/api/v1/acme-dns-compat",
    "allowfrom": []
  }
}
```

You can convert from a `credentials.json` file to a `lego-creds.json` file using:

``` bash
jq '{ (.fulldomain) : (.) }' credentials.json > lego-creds.json
```

This instructs LEGO to use your subdomain as the verification domain.

Protect these files as they contain secrets.


## Issue a certificate

``` yaml title="traefik.yml"
entryPoints:
  websecure:
    address: ":443"
certificatesResolvers:
  myresolver:
    acme:
      caServer: https://acme-staging-v02.api.letsencrypt.org/directory
      storage: /letsencrypt/acme.json
      dnsChallenge:
        provider: acme-dns
        delayBeforeCheck: 0
providers:
  docker:
    exposedByDefault: false
```

``` yaml title="docker-compose.yml"
version: '3'

services:

  # Traefik will proxy access to whoami and automate certificate management
  reverse-proxy:
    image: traefik:v2.10
    ports:
      - "443:443"
    environment:
      - ACME_DNS_API_BASE=https://api.getlocalcert.net/api/v1/acme-dns-compat
      - ACME_DNS_STORAGE_PATH=/creds.json
    volumes:
      - ./traefik.yml:/etc/traefik/traefik.yml
      - /var/run/docker.sock:/var/run/docker.sock
      - ./creds.json:/creds.json
      - ./letsencrypt:/letsencrypt

  # A basic upstream HTTP service, replace this with your actual service
  whoami:
    image: traefik/whoami
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.whoami.rule=Host(`YOUR_FQDN_HERE`)"
      - "traefik.http.routers.whoami.service=whoami"
      - "traefik.http.routers.whoami.entrypoints=websecure"
      - "traefik.http.routers.whoami.tls.certresolver=myresolver"
      - "traefik.http.services.whoami.loadbalancer.server.port=80"
```

First issue a certificate with Let's Encrypt Staging
(`caServer: https://acme-staging-v02.api.letsencrypt.org/directory`),
which allows you to test that you've configured everything correctly.
Once you're ready, switch to Let's Encrypt Production
(`caServer: https://acme-v02.api.letsencrypt.org/directory`),
or
[another CA](https://docs.getlocalcert.net/cas/zerossl/).

See also:

* Check out [traefik documentation](https://doc.traefik.io/traefik/https/acme/#dnschallenge) to learn more about configuring and deploying traefik.

You can see our integration test example [here](https://github.com/robalexdev/getlocalcert-client-tests/tree/main/examples/traefik).

[![Register and Issue using Traefik](https://github.com/robalexdev/getlocalcert-client-tests/actions/workflows/traefik.yml/badge.svg)](https://github.com/robalexdev/getlocalcert-client-tests/actions/workflows/traefik.yml)

