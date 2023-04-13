# cert-manager

[cert-manager](https://cert-manager.io/) is a certificate manager for Kubernetes and OpenShift.

!!! warning "Untested"

    This page needs review

## Issue a certificate

``` yaml
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: example-issuer
spec:
  acme:
    solvers:
    - dns01:
        acmeDNS:
          host: https://api.getlocalcert.net/api/v1/acme-dns-compat
          accountSecretRef:
            name: acme-dns
            key: credentials.json
```

See also:

* https://cert-manager.io/docs/configuration/acme/
* https://cert-manager.io/docs/configuration/acme/dns01/acme-dns/

