---
title: Caddy
summary: How to integrate Caddy with localcert.net
---

# Caddy

[Caddy](https://caddyserver.com/) is a modern web server with built-in automatic HTTPS support.

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

## Build Caddy

You'll need to customize your Caddy build to include the `dns.providers.acmedns` module.

1. go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest
2. xcaddy build --with github.com/caddy-dns/acmedns --with github.com/caddyserver/caddy/v2=github.com/caddyserver/caddy/v2@v2.6.4

Note: the second `--with` is a workaround for [a known issue](https://github.com/caddyserver/caddy/issues/5301).


## Set up a Caddyfile

As you begin, start with Let's Encrypt's staging environment as the `ca`.
Let's Encrypt's production environment has [rate limits](https://letsencrypt.org/docs/rate-limits/), so it's best to avoid using it until you've tested in the staging environment.

If you're using split view DNS, set `resolvers` to an external DNS server (like Google's `8.8.8.8:53`) or the localcert.net DNS server (`ns1.localcert.net`).
Otherwise, Caddy won't be able to see that the TXT records have been set and it won't proceed with certificate issuance.
If you're not using split view DNS, you can skip that line.

``` text title="Caddyfile"
<yourSubdomain>.localhostcert.net {
  tls {
    ca https://acme-staging-v02.api.letsencrypt.org/directory
    dns acmedns creds.json
  }
  respond "Hello from Caddy"
}
```

``` shell
sudo ./caddy run
```

Replace `<yourSubdomain>` with your subdomain name.

Now run: `sudo ./caddy run` to start your web server.
Check the logs to confirm that Let's Encrypt staging was able to issue you a certificate.
Load `yourSubdomain`.localhostcert.net in your web browser; you should see a certificate warning message.
This is expected as Let's Encrypt staging is not trusted by your browser.
You'll need to connect from a web browser on the same machine, as `yourSubdomain`.localhostcert.net will resolve to `127.0.0.1`.

Finally, remove or comment out (`#`) the `ca https://acme-staging-v02.api.letsencrypt.org/directory` line to switch the Let's Encrypt's production environment.
Run `sudo ./caddy run` again to issue a certificate.
Now when you connect to `yourSubdomain`.localhostcert.net you should no longer see a certificate warning message.

Caddy will manage your HTTPS certificate for you, automatically renewing your certificates before they expires.

Check out the [Caddy docs](https://caddyserver.com/docs/) for more information about configuring and deploying Caddy.

You can see our integration test example [here](https://github.com/robalexdev/getlocalcert-client-tests/tree/main/examples/caddy).

[![Register and Issue using Caddy](https://github.com/robalexdev/getlocalcert-client-tests/actions/workflows/caddy.yml/badge.svg)](https://github.com/robalexdev/getlocalcert-client-tests/actions/workflows/caddy.yml)

