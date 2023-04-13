# External Validation Domains

## The problem

If you're trying to issue certificates for a domain you own using the ACME DNS-01 protocol, you may find that your existing DNS server integrates poorly with ACME client tooling.
Some DNS servers don't have APIs and those that do may provide overly broad permissions (a security concern).

## The acme-challenge CNAME record

The ACME DNS-01 protocol allows a domain to set a `_acme-challenge` `CNAME` record instead of a `TXT` record.
The CNAME record should point to a different domain, such as one managed by getlocalcert.
This external validation domain will host the `TXT` record containing the challenge response.
With this setup, the existing DNS server only needs to manage a static `CNAME` record and the latter DNS server's API is used to dynamically update the `TXT` record each time a certificate is issued.

This `CNAME` record only impacts the `_acme-challenge` record.
`A`, `AAAA`, and any other records are not affected.

## Getting started

Let's say you'd like getlocalcert to help you issue certificates for a domain you own, like example.com.
First, create an account on getlocalcert and create a free domain name.
Create an API key for your newly created domain, and record the full domain name, API key, and API secret somewhere safe.
Next, log into the DNS server you use to manage DNS records for example.com.
Create a `CNAME` record like:

    _acme-challenge.example.com.    CNAME    yourSubdomain.localhostcert.net.

Finally, configure your ACME client with your getlocalcert credentials, domain name, and external domain name.
Automate renewal and you're all set.

## Security

You need to trust any DNS server you use to provide strong security properties.
When using an external validation domain, a security issue could allow an attacker to trick a CA into issuing certificates for your domain.
getlocalcert takes a [security-first](/security/) approach to protect against attack, but there's additional steps you can take to further protect your systems.

## CAA records

A CAA record contains a policy telling certificate authorities how they are allowed to issue certificates for a domain.
For example, a CAA record can [restrict certificate issuance](https://letsencrypt.org/docs/caa/) to a single certificate authority, or even a single [account](https://community.letsencrypt.org/t/enabling-acme-caa-account-and-method-binding/189588).

If you're using Let's Encrypt, follow the docs to determine your account's unique URL.
A full CAA record should look like:

    example.com.    CAA    0 issue "letsencrypt.org; accounturi=https://acme-v02.api.letsencrypt.org/acme/acct/123456789"

Once configured, your CAA record will protect you from security issues at your external validation domain.
This is sort of like two factor authentication for certificate issuance.


## Certificate transparency logs

Additionally, you can [monitor](/security/#Monitor-certificate-transparency-logs) certificate transparency logs to detect any misissuance of certificates for your domain.
If you quickly detect a misissued certificate, you can quickly revoke it and investigate.

## Self-hosting

If you'd prefer to do it yourself, you can self-host a DNS server like [acme-dns](https://github.com/joohoi/acme-dns/), which was purpose built to simplify the ACME DNS-01 process.

## See also

The EFF blog posted a [wonderful article](https://www.eff.org/deeplinks/2018/02/technical-deep-dive-securing-automation-acme-dns-challenge-validation) explaining these concerns.

