# Security

## HTTPS only

Our domains are pending addition to the [HSTS pre-load list](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security).
HSTS instructs the web browser to require HTTPS when connecting.
Invalid, self-signed, or expired certificates will block the user from connecting.
Without HSTS a web browser may allow a user to decide if they want to connect anyway.
With HSTS, there is no option to click through any certificate error messages.

getlocalcert helps automate certificate issuance, so you can minimize the risk of your users seeing an expired certificate.

## Scoped API keys

Each web application will need an API key to manage its `_acme-challenge` TXT record, so it may renew its certificate.
While many DNS APIs provide API keys with broad scope, getlocalcert API keys are scoped to a single subdomain.
This ensures that a security issue compromising the API keys of a single web application you manage cannot turn into a larger takeover of your other DNS records.

## Public Suffix List

Our domains are pending addition to the [Public Suffix List](https://publicsuffix.org/), which is the main tool used to identify which parts of a fully qualified domain name are public and which are available for registration to other parties.

Once our domains have been added to the list and the updated list has been rolled out to related tools, these domains will experience security and usability benefits. You can learn more at the [Public Suffix List website](https://publicsuffix.org/).

## Spam Protection

Local-only domains should not be able to send email to public email servers.
To prevent mail from these domains SPF, DKIM, and DMARC records are configured which instruct mail servers to reject any mail from our local-only domains.


!!! warning "Local Mail"
    
    If you need to send mail on your local network from these domains:

    * Configure your local network DNS server to override these records.
    * Allow-list your local-only domain on your mail server
    * Send mail from a different domain

