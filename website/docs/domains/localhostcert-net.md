---
title: Set up localhostcert.net domains
summary: How to set up localhostcert.net domains
---

# .localhostcert.net

## Use Case

* You are testing a web application and you'd like to see how it behaves using HTTPS
* You don't want to use a self-signed certificate, or setup a private Certificate Authority
* You don't want to route your (unencrypted) traffic through a third-party service

## How it works

You can register a free domain under `.localhostcert.net`.
Free domains use short auto-generated names, like `abc.localhostcert.net`.

Registration of a domain allows you to set the `_acme-challenge` TXT record for the domain.
These records are used to issue TLS certificates using the ACME DNS-01 protocol.
You should use an [ACME client](/acme-clients/) to set these records.

Unlike `.localcert.net`, `.localhostcert.net` has an `A` record set to `127.0.0.1` and an `AAAA` record set to `::1`.
You are not able to change these records, or set any other record types.

Locking the domain to localhost is designed to make it easier to quickly fetch a domain name, issue a certificate, and begin using it locally for software development.

