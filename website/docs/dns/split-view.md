---
title: Set up split-DNS with getlocalcert.net
summary: How to configure split-DNS with getlocalcert.net domains
---

# Split-View DNS

Split-view DNS involves an external DNS server and an internal DNS server which return different responses.
This is known as [split-view DNS](https://en.wikipedia.org/wiki/Split-view_DNS).
The external DNS server is provided by getlocalcert.
A second, internal DNS server can be configured on your internal network.
You may already have an internal DNS server, or you can set up one of your own.
This internal DNS server should handle routing for all local-only domain names.

A key benefit of this approach is that it ensures your subdomain names and IP addresses aren't leaked through DNS.
This is a form of [security through obscurity](https://en.wikipedia.org/wiki/Security_through_obscurity)

Another challenge, is that a user who switches between networks will continue to use DNS settings for the previous network due to DNS caching on their system.
This can cause a user who just connected to the internal network, perhaps via WiFi or VPN, to be unable to connect to the local-only domain.
Similarly, a user who just disconnected from the internal network and is now on a different network will still try to connect to the internal website using the previously seen IP address.
They'll usually see a failure to connect, although if a different internal website is listening on the same IP address of their new network, they will see a certificate error message.
Most browsers will not allow users to click through the certificate error message as our domains are present on the HSTS pre-load list.
After a few minutes the DNS cache should expire and the behavior should return to normal.
Some VPN clients will forcibly clear DNS caches to avoid this problem, and users can manually trigger a DNS flush if needed.

If your use case includes devices that frequently change networks, or your VPN client software is not configured to clear the DNS cache, you may wish to consider [managed DNS](/dns/managed/).

