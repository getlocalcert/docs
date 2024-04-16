---
title: FAQ
summary: Frequently asked questions (FAQ) about getlocalcert.net
---

# Frequently Asked Questions

## Why am I getting rate-limited by Let's Encrypt?

**Updated!**
This should occur less frequently now.
We've worked with the Let's Encrypt team to increase our rate limits and we're now on the Public Suffix List so each customer domain should have its own independent rate limits.

When testing out certificate issuance, it's best to start with the Let's Encrypt staging environment to avoid exhausting your rate limit.
The Let's Encrypt production environment has strict rate limits.

If you're still seeing problems, try using a different certificate authority, like [ZeroSSL](https://zerossl.com?fpr=getlocalcert&fp_sid=glcfaq)[^1].
You'll want to sign up for a free account, and then follow the [ZeroSSL instructions](/cas/zerossl/).

[^1]: The above link is an affiliate link.  If you purchase a paid ZeroSSL plan, we'll get a referral commission.

