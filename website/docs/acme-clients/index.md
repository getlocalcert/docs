# General guidance

getlocalcert domains support the ACME DNS-01 protocol for certificate issuance.
There's many ACME clients you can use, the general process is described here.

getlocalcert is not compatible with ACME HTTP-01 or TLS-ALPN-01, so it cannot work with ACME clients that only support these protocols.

## API keys

Certificate Authorities supporting ACME DNS-01 will issue you an HTTPS certificate if you can set a specific TXT record to a provided value.
The getlocalcert API uses API keys that are locked to specific subdomains.
As such, each application should manage it's TLS certificates using an API key scoped to the application's subdomain name.

If you haven't already, setup an API key for your subdomain in the console.

Several ACME clients support reading credentials from a JSON file.
You can create one like:

  {
    "username": "<yourApiKeyId>",
    "password": "<yourApiKeySecret>",
    "fulldomain": "<subdomain>.localhostcert.net",
    "subdomain": "<subdomain>",
    "server_url": "https://api.getlocalcert.net/api/acmedns/v1",
    "allowfrom": []
  }

## Setting TXT records

The getlocalcert API is compatible with the [acme-dns](https://github.com/joohoi/acme-dns) API for managing TXT records.
If your client is not 




Clients like [LEGO](./lego/) and [acme.sh](./acme-sh/) can be used to manage records.
Web servers like [Caddy](./caddy/) have built in support.

getlocalcert limits how many TXT records you may have, so delete TXT records once your certificate have been signed (you don't need to keep them any longer).

