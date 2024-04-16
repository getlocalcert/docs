---
title: Security
summary: Security features and security model of getlocalcert.net
---

# Security

## Designed for security

Here are several ways getlocalcert's design protects your domain.

### Scoped API keys

Every web application using ACME DNS-01 will need an API key to update its `_acme-challenge` TXT record during the certificate issuance process.
While many DNS APIs provide API keys with an overly broad scope, getlocalcert API keys are scoped to a single subdomain and may only update the `_acme-challenge` TXT record.
This ensures that a security issue compromising the API keys of a single web application you manage cannot turn into a larger takeover of your other DNS records or other domains.

getlocalcert implements granular permissions using the same API as [acme-dns](https://www.eff.org/deeplinks/2018/02/technical-deep-dive-securing-automation-acme-dns-challenge-validation).
As such, many tools that support acme-dns also support getlocalcert.

### Public Suffix List

Our domains are part of the [Public Suffix List](https://publicsuffix.org/).

A public suffix is any domain that allows registration by independent parties.
These include:

* top-level domains like `.com`
* second-level domains like `.co.uk`
* privately controlled domains like `.localcert.net` and `.github.io`

You should avoid registering domains that aren't on the public suffix list due to cookie-handling concerns.
Web browsers use the public suffix list to determine if it's safe to share cookies through a parent domain.
This means that your domain (`example.localcert.net`) won't be able to set cookies on the parent domain (`localcert.net`) nor can other domains (`attacker.localcert.net`) read cookies on the parent domain.

You can learn more at the [Public Suffix List website](https://publicsuffix.org/).

### Spam Protection

Local-only domains should not be able to send email to public email servers.
To prevent mail from these domains SPF, DKIM, and DMARC records are configured which instruct mail servers to reject any mail from our local-only domains.
These records are configured automatically when a domain is created.


!!! warning "Local Mail"
    
    If you need to send mail on your local network from these domains:

    * Configure your local network DNS server to override these records.
    * Allow-list your local-only domain on your mail server
    * Send mail from a different domain


## Things you can do

### Monitor certificate transparency logs

When you use getlocalcert and the larger certificate authority ecosystem, you may be worried about certificate misissuance.
The Internet depends on the trustworthiness and security practices of the CAs, and there's a history of problems.
Thankfully, all commonly trusted public CAs provide certificate transparency logs that contain information about every certificate they issue.
These logs have helped security professionals detect misissuance and mitigate real-world attacks.

If you'd like to keep an eye on things, you can sign up for a [certificate monitoring service](https://certificate.transparency.dev/monitors/).
These services track every certificate issuance and can notify you each time a certificate is issued for your domain.

### CAA records for delegated domains

If you're using getlocalcert as an [external validation domain](/tips/validation-domain/) consider setting a CAA record.
Let's Encrypt supports CAA records that restrict certificate issuance to a single Let's Encrypt account.
This works sort of like two-factor authentication, you'll need credentials for your Let's Encrypt account and an API key to update your DNS records.


### HSTS headers

Set [HSTS headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security) on your web server.
HSTS instructs the web browser to require HTTPS when connecting.
Invalid, self-signed, or expired certificates will block the user from connecting.

