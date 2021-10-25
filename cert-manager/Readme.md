
### ClusterIssuer:

```sh
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
  # name: letsencrypt-production
  namespace: default
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    # server: https://acme-v02.api.letsencrypt.org/directory
    email: <YOUR_EMAIL>
    privateKeySecretRef:
      name: letsencrypt-staging
      # name: letsencrypt-production
    solvers:
    - selector: {}
      http01:
        ingress:
          class: traefik
```

### Cetificate :

```sh
apiVersion: cert-manager.io/v1
kind: Certificate
metadata:
  name: ssl-cert-staging
  namespace: default
spec:
  secretName: ssl-cert-staging
  issuerRef:
    name: letsencrypt-staging
    kind: ClusterIssuer
  commonName: <YOUR_IP>.nip.io
  dnsNames:
  - <YOUR_IP>.nip.io
```

