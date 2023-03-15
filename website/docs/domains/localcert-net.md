# .localcert.net

!!! warning "Unreleased"
    
    This feature has not been released yet.

## Use Case

* You are running an private, internal web service that's only accessible on your local machine, local network, virtual private network, or other non-Internet network.
* You'd like to deploy HTTPS
* You don't want users to see warnings about self-signed certificates
* You don't want users to need to install and trust a private Certificate Authority root cert

## How it works

You can register a free domain under `.localcert.net`.
Free domains use short auto-generated names, like `abc.localcert.net`.

Registration of a domain allows you to set the `_acme-challenge` TXT record for the domain.
These records are used to issue TLS certificates using the ACME DNS-01 protocol.
You should use an [ACME client](/acme-clients/) to set these records.

You are unable to set any other public DNS records, such as `A`, `AAAA`, `MX`, `CNAME`, or `NS` records.
This prevents your domain from hosting any Internet-facing content using the public, global DNS.

You are encouraged to set DNS records on any private DNS server you operate, such as your corporate DNS server, or home Internet router.
Any system on your private network, using your internal DNS server, will be able to connect to your internal web application.
You may also use other methods, like the `/etc/hosts` file, hard-coding IP addresses in apps, or custom service discovery systems if you'd prefer.

