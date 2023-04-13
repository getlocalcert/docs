# acme.sh

[acme.sh](https://github.com/acmesh-official/acme.sh) is an ACME client written in bash.

## API Keys

If you haven't already, setup an API key for your subdomain in [the console](https://console.getlocalcert.net/).

## Issue a certificate

As you begin, start with Let's Encrypt's staging environment (`--staging`).
Let's Encrypt's production environment has [rate limits](https://letsencrypt.org/docs/rate-limits/), so it's best to avoid using it until you've tested in the staging environment.

``` bash
export ACMEDNS_BASE_URL="https://api.getlocalcert.net/api/v1/acme-dns-compat"
export ACMEDNS_USERNAME="yourApiKeyId"
export ACMEDNS_PASSWORD="yourApiKeySecret"
export ACMEDNS_SUBDOMAIN="yourSubdomain"

acme.sh --issue \
        --dns dns_acmedns \
        -d <yourSubdomain>.localhostcert.net \
        --staging
```

If everything succeeded, you'll see that a certificate was issued.
You can now run again without the `--staging` argument to use the Let's Encrypt production environment.

Checkout the [acme.sh docs](https://github.com/acmesh-official/acme.sh/wiki)
for more information about copying these certificates to your web server and automating certificate renewals.

