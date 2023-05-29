---
title: Let's Encrypt Go (LEGO)
summary: How to integrate Let's Encrypt Go (LEGO) with getlocalcert.net
---

# LEGO

[LEGO](https://github.com/go-acme/lego) is a Let's Encrypt ACME client written in go.

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


## Register a subdomain and issue a certificate

As you begin, start with Let's Encrypt's staging environment as the `--server`.
Let's Encrypt's production environment has [rate limits](https://letsencrypt.org/docs/rate-limits/), so it's best to avoid using it until you've tested in the staging environment.
The following command issues a certificate that's valid for both your subdomain and all child subdomains (I.E. a wildcard certificate).

``` bash
export ACME_DNS_API_BASE=https://api.getlocalcert.net/api/v1/acme-dns-compat
export ACMEDNS_FULLDOMAIN=subdomain.localhostcert.net
export ACMEDNS_EMAIL=me@example.com
export ACME_DNS_STORAGE_PATH=creds.json

./lego \
  --accept-tos \
  --email ${ACMEDNS_EMAIL} \
  --dns acme-dns \
  --domains ${ACMEDNS_FULLDOMAIN} \
  --domains *.${ACMEDNS_FULLDOMAIN} \
  --server https://acme-staging-v02.api.letsencrypt.org/directory \
  run
```

If everything succeeded, you'll see that a certificate was issued.
You can now run again without the `--server` argument to use the Let's Encrypt production environment.

Check out the [LEGO docs](https://go-acme.github.io/lego/) for more information about copying these certificates to your web server and automating certificate renewals.

You can see our integration test example [here](https://github.com/robalexdev/getlocalcert-client-tests/blob/main/examples/lego/register-and-issue.sh).

[![Register and Issue using LEGO](https://github.com/robalexdev/getlocalcert-client-tests/actions/workflows/lego.yml/badge.svg)](https://github.com/robalexdev/getlocalcert-client-tests/actions/workflows/lego.yml)

