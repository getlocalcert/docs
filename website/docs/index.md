---
title: Documentation
summary: This is the documentation site for localcert.net
---

# Migration
We are currently in a migration period, where we are moving DNS resources to Cloudflare away from a self hosted instance of PowerDNS. DNS records may not deploy properly until we have completed this migration.

If you experience any issues during this period, please email [support@localcert.net](mailto:support@localcert.net) and we will try our best to assist you.

# About

**Get valid HTTPS certificates for your local network in just ten minutes!**

[localcert.net](https://localcert.net) lets you easily deploy Let's Encrypt HTTPS certificates to internal web apps.

[Get started for free](https://console.getlocalcert.net/){ .md-button .md-button--primary }

Register a local-only domain name and automate issuance of domain verified certificates.
Paired with automatic certificate management (ACME), internal websites can easily renew their certificates without manual process.

## Free local-only domain names

Our free local-only domain can only be used on a private network.
getlocalcert restricts how a registrant can manage DNS for their domain.
`A`, `AAAA`, and `CNAME` records are handled differently on each of our domains.
Other records, like `MX` and `NS`, may not be set.

## Issuing Certificates

The registrant is able to manage `TXT` records with the name `_acme-challenge`.
This record is used during the Automated Certificate Management (ACME) DNS-01 protocol when issuing a domain verified certificate.
By allowing the registrant to control this record, we support TLS certificate issuance even for systems that are not accessible from the Internet.

Free certificates may be obtained from Let's Encrypt.
