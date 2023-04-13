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

Protect this file as it contains a secret key.

## Issue a certificate

As you begin, start with Let's Encrypt's staging environment as the `--server`.
Let's Encrypt's production environment has [rate limits](https://letsencrypt.org/docs/rate-limits/), so it's best to avoid using it until you've tested in the staging environment.

``` bash
export ACME_DNS_STORAGE_PATH=credentials.json
export ACME_DNS_API_BASE=https://api.getlocalcert.net/api/v1/acme-dns-compat
lego --accept-tos \
     --email you@example.com \
     --dns acme-dns \
     --domains <yourSubdomain>.localhostcert.net \
     --server https://acme-staging-v02.api.letsencrypt.org/directory \
     --dns.resolvers 8.8.8.8
     run
```

TODO: This tries to register an account, even though credentials.json has a value.

Replace `<yourSubdomain>` with your subdomain name.

If everything succeeded, you'll see that a certificate was issued.
You can now run again without the `--server` argument to use the Let's Encrypt production environment.

Checkout the [LEGO docs](https://go-acme.github.io/lego/) for more information about copying these certificates to your web server and automating certificate renewals.

