# General guidance

There's many ACME clients you can use, the general process is described here.
getlocalcert domains supports the ACME DNS-01 protocol for certificate issuance.

## API keys

If you haven't already, setup an API key for your subdomain in [the console](https://console.getlocalcert.net/).
The getlocalcert API uses API keys that are locked to a single subdomain.
You'll use your API key to update a DNS record as part of the certificate issuance process.

Several ACME clients support reading credentials from a JSON file.
You can create one like:

``` json title="credentials.json"
{
  "username": "<yourApiKeyId>",
  "password": "<yourApiKeySecret>",
  "fulldomain": "<yourSubdomain>.localhostcert.net",
  "subdomain": "<yourSubdomain>",
  "server_url": "https://api.getlocalcert.net/api/acmedns/v1",
  "allowfrom": []
}
```

Protect this file as it contains a secret key.

## Setting TXT records

The getlocalcert API is compatible with the [acme-dns](https://github.com/joohoi/acme-dns) API for managing TXT records.
Many ACME clients are compatible with this API.
See the sidebar for specific instructions for several tools.

You can also set the ACME DNS-01 challenge response record manually through the getlocalcert web console, although you'll have a much better experience using an existing automated client.

