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

Install cert-manager in your cluster, then configure an issuer and a certificate.
Edit the manifest below to include your email address in `email`
and the domain name you registered in `dnsNames`.

``` yaml title="my-certificate.yaml"
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: le-staging-issuer
spec:
  acme:
    email: me@example.com
    privateKeySecretRef:
      name: cert-manager-demo-private-key
    # Change this to Let's Encrypt production once you've validated your setup
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    solvers:
    - dns01:
        acmeDNS:
          host: https://api.getlocalcert.net/api/v1/acme-dns-compat
          accountSecretRef:
            name: acme-dns
            key: cert-manager-creds.json
---
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: cert-manager-test
spec:
  dnsNames:
    - cert-manager-demo.localcert.net
    # A wildcard cert as well (use quotes)
    - "*.cert-manager-demo.localcert.net"
  secretName: cert-manager-test
  issuerRef:
    name: le-staging-issuer
```

!!! note

    The `cert-manager-creds.json` secret should go in the same namespace as the `Issuer` resource.
    If using a `ClusterIssuer`, then the `cert-manager-creds.json` secret should go in the same namespace as cert-manager.

Configure the credentials and install the resources:

    kubectl create secret generic acme-dns --from-file cert-manager-creds.json
    kubectl apply -f my-certificate.yaml

## Checking certificate status

You can check if the certificate was issued using:

    kubectl get certificate cert-manager-test

If you're seeing issues, check a few other resources as well:

    kubectl describe certificaterequest
    kubectl describe issuer le-staging-issuer
    kubectl describe challenge

## Let's Encrypt environments

First issue a certificate with Let's Encrypt Staging
(`server: https://acme-staging-v02.api.letsencrypt.org/directory`),
which allows you to test that you've configured everything correctly.
Once you're ready, switch to Let's Encrypt Production
(`server: https://acme-v02.api.letsencrypt.org/directory`),
or
[another CA](https://docs.getlocalcert.net/cas/zerossl/).

## See also

[![Register and Issue using cert-manager](https://github.com/robalexdev/getlocalcert-client-tests/actions/workflows/cert-manager.yml/badge.svg)](https://github.com/robalexdev/getlocalcert-client-tests/actions/workflows/cert-manager.yml)

* [Integration test example](https://github.com/robalexdev/getlocalcert-client-tests/tree/main/examples/cert-manager)
* https://cert-manager.io/docs/configuration/acme/
* https://cert-manager.io/docs/configuration/acme/dns01/acme-dns/

