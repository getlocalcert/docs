# About

**Get valid HTTPS certificates for your local network in just ten minutes!**

[getlocalcert.net](https://getlocalcert.net) lets you easily deploy Let's Encrypt HTTPS certificates to internal web apps.

!!! warning "Early Access"
    
    getlocalcert is currently available for early access.
    Feel free to try us out, but be aware that some breaking changes may occur and several known issues exist.

[Get started for free](#){ .md-button .md-button--primary }

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

