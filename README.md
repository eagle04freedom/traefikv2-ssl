# traefikv2-ssl
Traefikv2 + cert-manager + LetsEncrypt

## 1. Install Traefikv2
  * add below content to traefikcustom-values.yaml file along with the existing content of values-test.yaml.
  * Make sure to delete the fields 
```sh
ssl:
  enabled: true
api:
  insecure: true
#service:
#  spec:
#    loadBalancerIP: <external-ip>

additionalArguments:
  #- "--providers.kubernetesingress.ingressclass=traefik"
  - "--log.level=DEBUG"
  #- "--providers.kubernetesingress.namespaces=ingress,kdp"
```
