---
title: Managed DNS
summary: How to configure getlocalcert.net on managed DNS domains
---

!!! warning "Unreleased"

    This feature has not been released yet.

# Managed DNS

getlocalcert can act as the sole authoritative DNS server for your domain.
In this setup, you'll configure your DNS records on getlocalcert, and they'll be available publicly on the global DNS.

A key benefit of this approach is that you will not need to configure an internal DNS server or deal with any split-view DNS inconsistency issues.

One downside is that hosting your internal DNS records publicly will leak the subdomain names and internal IP address to external parties.
Generally, leaking internal IP addresses isn't a security problem as external parties cannot connect without first connecting to your internal network.
However, it may be considered a privacy concern.
You may want to consider avoiding sensitive subdomain names as these can leak in other places, such as certificate transparency logs.
If you need to have sensitive subdomain names, you may need to use split-view DNS and wildcard TLS certificates.

