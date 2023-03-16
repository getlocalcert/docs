# LEGO

[LEGO](https://github.com/go-acme/lego) is a Let's Encrypt ACME client written in go.

## API Keys

Make sure you have a getlocalcert API key for the domain you'd like to configure.
An API key will be created every time you set up a subdomain, or manually as you need.

## Issue a certificate

Let's say you'd like to issue a new certificate for the `hostname` subdomain of your `abc.localcert.net` domain.

First, setup a credentials.json file:

``` json title="credentials.json"
{
    "fulldomain": "hostname.abc.localcert.net",
    "subdomain" : "hostname",
    "username"  : "...",
    "password"  : "...",
}
```

Where `username` is your API key ID and `password` is the API key secret.
Protect this file as it contains a secret key.

Finally, set some environmental variables and run `lego`:

``` bash
export ACME_DNS_STORAGE_PATH=path/to/credentials.json
export ACME_DNS_API_BASE=https://console.getlocalcert.net/acmedns-api-v1/

lego --email you@example.com --dns acme-dns --domains hostname.abc.localcert.net run
```




