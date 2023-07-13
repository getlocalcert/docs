---
title: cert-manager
summary: How to integrate cert-manager with getlocalcert.net
---

# cert-manager

[cert-manager](https://cert-manager.io/) is a certificate manager for Kubernetes and OpenShift.

## API Keys

If you haven't already, setup an API key for your subdomain in [the console](https://console.getlocalcert.net/).
Save your subdomain information and credentials to a JSON file like this:

``` json title="credentials.json"
{
  "username": "<yourApiKeyId>",
  "password": "<yourApiKeySecret>",
  "fulldomain": "<yourSubdomain>.localhostcert.net",
  "subdomain": "<yourSubdomain>",
  "server_url": "https://api.getlocalcert.net/api/v1/acme-dns-compat",
  "allowfrom": []
}
```

cert-manager uses a slightly different format to manage these keys:

``` json title="cert-manager-creds.json"
{
  "<yourSubdomain>.localhostcert.net": {
    "username": "<yourApiKeyId>",
    "password": "<yourApiKeySecret>",
    "fulldomain": "<yourSubdomain>.localhostcert.net",
    "subdomain": "<yourSubdomain>",
    "server_url": "https://api.getlocalcert.net/api/v1/acme-dns-compat",
    "allowfrom": []
  }
}
```

You can convert from a `credentials.json` file to a `cert-manager-creds.json` file using:

``` bash
jq '{ (.fulldomain) : (.) }' credentials.json > cert-manager-creds.json
```

This instructs cert-manager to use your subdomain as the verification domain.

Protect these files as they contain secrets.

## Issue a certificate

``` yaml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: example-issuer
spec:
  acme:
    email: <email for notification>
    privateKeySecretRef:
      name: <name of a secret used to store the ACME account private key>
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    solvers:
    - dns01:
        acmeDNS:
          host: https://api.getlocalcert.net/api/v1/acme-dns-compat
          accountSecretRef:
            name: acme-dns
            key: cert-manager-creds.json
```

!!! note

    The `cert-manager-creds.json` secret should go in the same namespace as the `Issuer` resource.
    If using a `ClusterIssuer`, then the `cert-manager-creds.json` secret should go in the same namespace as cert-manager.


First issue a certificate with Let's Encrypt Staging
(`server: https://acme-staging-v02.api.letsencrypt.org/directory`),
which allows you to test that you've configured everything correctly.
Once you're ready, switch to Let's Encrypt Production
(`server: https://acme-v02.api.letsencrypt.org/directory`),
or
[another CA](https://docs.getlocalcert.net/cas/zerossl/).

See also:

* https://cert-manager.io/docs/configuration/acme/
* https://cert-manager.io/docs/configuration/acme/dns01/acme-dns/

