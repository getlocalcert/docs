---
title: Integrate Certify The Web with getlocalcert.net
summary: How to integrate Certify The web with getlocalcert.net
---

# Certify The Web

[Certify The Web](https://certifytheweb.com/) is certificate management and automation tool for IIS and Windows.

## Issue a certificate

1. Click *New Certificate* on the Identifiers tab, enter your domain, and
click the add button.
2. On the Authorization tab, select Challenge Type `dns-01`, select
`acme-dns` as the DNS update method.
3. Enter `https://api.getlocalcert.net/api/v1/acme-dns-compat` as the API Url
4. Paste your [JSON credentials file](/acme-clients/#json-credentials-file)
including your getlocalcert.net credentials.
This allows you to skip the acme-dns registration step the app would normally attempt.
5. Click *Request Certificate* to save your settings and perform a
certificate order.
6. If you have a compatible service like IIS configured on the same
machine with matching http or https bindings for your hostname the
certificate will be automatically applied. You can also use the Tasks
tab to configure deployment tasks for copying/applying your certificate
automatically.

These instructions apply to Certify The Web v6.0 onwards. Help is
available at https://community.certifytheweb.com/

See also:

* https://docs.certifytheweb.com/docs/dns/providers/acme-dns/

