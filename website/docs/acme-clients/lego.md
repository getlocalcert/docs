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

``` bash title="convert.sh"
echo '{'                        >  lego-creds.json
jq .fulldomain credentials.json >> lego-creds.json
echo -n ':'                     >> lego-creds.json
cat credentials.json            >> lego-creds.json
echo "}"                        >> lego-creds.json
```

This instructs LEGO to use your subdomain as the verification domain.

Protect these files as they contain secrets.


## Issue a certificate

As you begin, start with Let's Encrypt's staging environment as the `--server`.
Let's Encrypt's production environment has [rate limits](https://letsencrypt.org/docs/rate-limits/), so it's best to avoid using it until you've tested in the staging environment.
The following command issues a certificate that's valid for both your subdomain and all child subdomains (I.E. a wildcard certificate).

``` bash
export ACME_DNS_STORAGE_PATH=lego-creds.json
export ACME_DNS_API_BASE=https://api.getlocalcert.net/api/v1/acme-dns-compat
export ACME_DNS_FULLDOMAIN=$(jq .fulldomain credentials.json)
lego \
  --accept-tos \
  --email you@example.com \
  --dns acme-dns \
  --domains ${ACME_DNS_FULLDOMAIN} \
  --domains *.${ACME_DNS_FULLDOMAIN} \
  --server https://acme-staging-v02.api.letsencrypt.org/directory \
  run
```

If everything succeeded, you'll see that a certificate was issued.
You can now run again without the `--server` argument to use the Let's Encrypt production environment.

Checkout the [LEGO docs](https://go-acme.github.io/lego/) for more information about copying these certificates to your web server and automating certificate renewals.

