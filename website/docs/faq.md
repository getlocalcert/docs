# Frequently Asked Questions

## Why am I getting rate limited by Let's Encrypt?

When testing out certificate issuance, it's best to start with the Let's Encrypt staging environment.
The production environment has strict rate limiting, which you can easily exceed if you are not careful.

Additionally, while getlocalcert is in an early access status, the Let's Encrypt rate limit is shared by all subdomains.
This means someone else may have exceeded the shared rate limit.
Before getlocalcert reaches a general availability status, we'll get added to the public suffix list which, among other things, will allow each getlocalcert subdomain to have it's own independent rate limit.

Until then, try using a different certificate authority, like [ZeroSSL](https://zerossl.com?fpr=getlocalcert&fp_sid=glcfaq)[^1].
You'll want to sign up for a free account, then follow the [ZeroSSL instructions](/cas/zerossl/).


[^1]: The above link is an affiliate link.  If you purchase a paid ZeroSSL plan, we'll get a referral commission.






