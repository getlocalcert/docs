# acme.sh

[acme.sh](https://github.com/acmesh-official/acme.sh) is an ACME client written in bash.

## API Keys

Make sure you have a getlocalcert API key for the domain you'd like to configure.

## Issue a certificate

Let's say you'd like to issue a new certificate for the `hostname` subdomain of your `abc.localcert.net` domain.
You can run:

``` bash
export ACMEDNS_BASE_URL="https://console.getlocalcert.net/acmedns-api-v1/"
export ACMEDNS_USERNAME="..."
export ACMEDNS_PASSWORD="..."
export ACMEDNS_SUBDOMAIN="hostname"

./acme.sh --issue --dns dns_acmedns -d hostname.abc.localcert.net
```

Where `ACMEDNS_USERNAME` is your API key ID and `ACMEDNS_PASSWORD` is the API key secret.

