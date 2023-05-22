---
title: ACME-challenge delegates
summary: How to configure getlocalcert.net domains as ACME-challenge delegate subdomain
---

# ACME-challenge delegate subdomains

## The problem

If you're trying to issue certificates for a domain you own using the ACME DNS-01 protocol, you may find that your existing DNS server integrates poorly with ACME client tooling.
Many DNS servers have inadequate APIs or use API keys that provide overly broad permissions (a security concern).
You want your web server to be able to renew it's own certificate, but you don't want to give it control over all your DNS entries.

## The acme-challenge CNAME record

The ACME DNS-01 protocol allows a domain to solve the challenge using a `_acme-challenge` `CNAME` record instead of the usual `TXT` record.
The CNAME record should point to a different domain, such as one managed by getlocalcert.
The pointed-to domain is known as an "ACME-challenge delegate" and it will host the `TXT` record containing the challenge response.
With this setup, the existing DNS server only needs to manage a static `CNAME` record and the latter DNS server's API is used to dynamically update the `TXT` record each time a certificate is issued.

This `CNAME` record only impacts the `_acme-challenge` record.
`A`, `AAAA`, and any other records are not affected.

getlocalcert helps you set up a ACME-challenge delegate and provides API keys with minimal permission.

## Quick start

You can use [LEGO](https://github.com/go-acme/lego) to quickly set up you ACME-challenge delegate.
Replace example.com with your domain name and run:

```
export ACME_DNS_STORAGE_PATH=lego-creds.json
export ACME_DNS_API_BASE=https://api.getlocalcert.net/api/v1/acme-dns-compat
lego \
  --accept-tos \
  --email you@example.com \
  --dns acme-dns \
  --domains example.com \
  --server https://acme-staging-v02.api.letsencrypt.org/directory \
  run
```

LEGO will automatically register a ACME-challenge delegate domain for you.
Information about the delegate domain, including the API key, are saved to lego-creds.json.
The first time you run LEGO it will prompt you to add a CNAME record (`you must provision the following CNAME in your DNS zone and re-run this provider when it is in place`).
This is a one-time manual process to permit your domain to use the newly issued delegate for certificate issuance.
Once your CNAME record is configured, run the command again.

You may see some timeouts while waiting for DNS record propagation.
These can happen when your DNS resolver has cached a stale TXT record.
If you don't want to wait you can try:

* switching to a different resolver with the `--dns.resolvers 1.1.1.1` flag; or
* disabling the LEGO's propagation check with `--dns.disable-cp`

If everything goes well you'll have a Let's Encrypt staging certificate for your domain.
Finally, you can remove the `--server` flag to switch to Let's Encrypt production.

Follow the [LEGO docs](https://go-acme.github.io/lego/) to automate certificate renewal and copying the certificates to your web server.


## General Guidance

Let's say you'd like to issue certificates for `example.com` using the ACME DNS-01 process.
You can use any ACME client that supports the ACME DNS-01 protocol, especially where the acme-dns DNS server is supported.

First, create a free subdomain on getlocalcert to use as the delegate.
Create an API key for your newly created domain, and record the full domain name, API key, and API secret somewhere safe.

Next, add a `CNAME` record for `_acme-challenge` on example.com using your existing DNS provider.
For example, if you registered your domain using namecheap and use their DNS service, add the CNAME using the namecheap web console:

    _acme-challenge.example.com.    CNAME    yourSubdomain.localhostcert.net.

You'll need to ask your ACME client to issue a certificate for example.com (and \*.example.com if you'd like), but provide the delegate domain in the DNS settings.
Always start with the Let's Encrypt staging environment or [ZeroSSL](/cas/zerossl) to avoid getting rate limited.

Check your ACME client's docs to configure automatic renewals and deploy the certificates to your web server.

## Security

You need to trust any DNS server you use to provide strong security properties.
When using an ACME-challenge delegate, a security issue could allow an attacker to trick a CA into issuing certificates for your domain.
getlocalcert takes a [security-first](/security/) approach to protect against attack, but there's additional steps you can take to further protect your systems.

## CAA records

A CAA record contains a policy telling certificate authorities how they are allowed to issue certificates for a domain.
For example, a CAA record can [restrict certificate issuance](https://letsencrypt.org/docs/caa/) to a single certificate authority, or even a single [account](https://community.letsencrypt.org/t/enabling-acme-caa-account-and-method-binding/189588).

If you're using Let's Encrypt, follow the docs to determine your account's unique URL.
A full CAA record should look like:

    example.com.    CAA    0 issue "letsencrypt.org; accounturi=https://acme-v02.api.letsencrypt.org/acme/acct/123456789"

Once configured, your CAA record will protect you from security issues at your ACME-challenge delegate domain.
This is sort of like two factor authentication for certificate issuance.


## Certificate transparency logs

Additionally, you can [monitor](/security/#Monitor-certificate-transparency-logs) certificate transparency logs to detect any misissuance of certificates for your domain.
If you detect a misissued certificate, you can quickly revoke it and investigate.

## Self-hosting

If you'd prefer to do it yourself, you can self-host a DNS server like [acme-dns](https://github.com/joohoi/acme-dns/), which was purpose built to simplify the ACME DNS-01 process.

## See also

The EFF blog posted a [wonderful article](https://www.eff.org/deeplinks/2018/02/technical-deep-dive-securing-automation-acme-dns-challenge-validation) explaining these concerns.

