# General guidance

getlocalcert domains only support the ACME DNS-01 protocol for certificate issuance.
While there's many ACME clients you can use, many can be configured using the instructions below.

getlocalcert is not compatible with ACME HTTP-01 or TLS-ALPN-01, so it cannot work with ACME clients that only support these protocols.

## API keys

Fundamentally, CAs supporting ACME DNS-01 will issue a TLS certificate to anyone who can set a specific TXT record to a provided value.
The getlocalcert API uses API keys that are locked to specific subdomains.
As such, each application should manage it's TLS certificates using an API key scoped to the application's subdomain name.

## Setting TXT records

The getlocalcert API is compatible with the ACMEDNS API for managing TXT records.
Clients like [LEGO](TODO) and [acme.sh](TODO) can be used to manage records.

You'll need to set:
* endpoint: apiv1.getlocalcert.net
* username: Your API Key ID
* password: Your API Secret Key
* subdomain: I.E.: host.abc.localcert.net

getlocalcert limits how many TXT records you may have, so delete TXT records once your certificate have been signed (you don't need to keep them any longer).

