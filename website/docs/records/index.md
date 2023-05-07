---
title: Default records
summary: Information about default DNS records on getlocalcert.net domains
---

# Default Records

getlocalcert initializes your subdomain with several default DNS records.
These are helpful to ensure that the getlocalcert service works well for private networks.
Many of these cannot be modified.


## A records

Subdomains of localhostcert.net are configured with an `A` record set to `127.0.0.1`.
This record cannot be modified as localhostcert.net is designed for localhost use only.

However, you may use [split view DNS](/dns/split-view/) to set your own A records internal to your network.


## Email records

Private domains cannot be send or receive email.
To prevent spam, several email related DNS records have been set and cannot be modified.

### SPF

Sender policy framework (SPF) indicates which servers are authorized to send email.
getlocalcert sets a null SPF record, indicating that no servers are authorized senders.

### DKIM

DomainKeys Identified Mail (DKIM) uses digital signatures to authenticate email.
getlocalcert sets a null DKIM record, indicating that there is no valid signing key.

### DMARC

Domain-based Message Authentication Reporting and Conformance (DMARC) specifies a policy for authenticating email.
getlocalcert sets a DMARC record that indicates that any email failing the SPF or DKIM check should be rejected.
Paired with the null SPF and null DKIM policy, any modern mail server will reject any spoofed email for managed domains.

### MX

The MX record indicates the server used to receive email for a domain.
getlocalcert sets a null MX record, indicating that managed subdomains cannot receive email.


## NS records

NS records indicate which DNS server is authoritative for a domain.
The getlocalcert DNS server is authoritative for all managed subdomains.
NS records cannot be modified.


## SOA records

The SOA (start of authority) record contains technical information used to manage the subdomain.
This record cannot be modified.

