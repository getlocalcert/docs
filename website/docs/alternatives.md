# Alternatives

## Why not self-signed certificates?

Self-signed certificates are easy to set up, but provide poor user experience.
When a user connects to a website using a self-signed certificate they'll see a warning message.
Generally, the user can click through the warning message to proceed.

There's a number of problems here.
First, the user won't be able to distinguish a legitimate self-signed certificate from one created maliciously.
Users will click through the warning message in both cases, as they don't have a way to validate the connection.
Second, users learn that clicking through certificate warning messages is an acceptable work-around.
This means that users will be less suspicious if they see a self-signed certificate on other websites, like their bank's website.
Clicking through those certificate warning messages could cause grave harm, including loss of money, loss of sensitive data, or system compromise.

## Why not private CAs?

Setting up a private CA is a popular improvement to self-signed certificates.
When properly configured the user should no longer see certificate warning messages.

Unfortunately, this isn't a perfect solution either.
Every operating system and web browser trust store must be updated to include the private CA.
This can be a large amount of on-going work, especially for organizations with bring-your-own-device policies.
If you miss any, then the user will see a warning message and you're back to the same issues you had with self-signed certificates.
Even worse, a private CA is able to sign certificates for any domain name, even ones you don't own.
This means that a hacker or disgruntled employee with access to the private CA can forge TLS certificates for any domain.

Further, user-imported CA certs are handled in inconsistent and surprising ways.
The same browser may behave differently on different operating systems, based on OS-level TLS feature support.
For example, a user-imported CA cert is [trusted past it's expiration](https://bugs.chromium.org/p/chromium/issues/detail?id=1072083&q=trust%20root%20expiration&can=2) on Chrome, by design.
Feature support like RFC 5280 "Name Constraints", which seeks to scope a CA cert to a specific namespace, is [poor but growing](https://netflixtechblog.com/bettertls-c9915cd255c0?gi=d6fcf982d4ac).
Tooling for creating CA certs are often [missing important security features](https://github.com/FiloSottile/mkcert/issues/302).

## Bring your own domain

Using a domain or subdomain you own is the best option.
You'll be able to issue HTTPS certificates for your domain through a widely trusted CA, like the free Let's Encrypt service.
You users will require no additional set up as Let's Encrypt is widely trust across all popular web browsers and operating systems.

getlocalcert helps you achieve these benefits through our free and bring-your-own domain offerings.

