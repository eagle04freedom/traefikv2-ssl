# traefikv2-ssl
Traefikv2 + cert-manager + LetsEncrypt

## 1. Un-Install Traefikv2



## 2. Configure Pebble

### 2.1 Prepare pebble repo for installation

```sh
$ helm repo add jupyterhub https://jupyterhub.github.io/helm-chart/
$ helm repo update jupyterhub/pebble
$ helm repo search jupyterhub

# helm show values jupyterhub/pebble > pebble/pebble-values-org.yaml

$ helm pull jupyterhub/pebble --untar
```
* Edit the custom-values.yaml 

```sh
$ vim pebble/custom-values.yaml

env:
  - name: PEBBLE_VA_ALWAYS_VALID
    value: "1"
coredns:
  enabled: false

```

### 2.2 Install Pebbele in kdp namespace
```sh
$ helm install pebble jupyterhub/pebble -f pebble/custom-values.yaml -n kdp
```

> take screenshot of few output after installation
 
## 3. Traefik:

### 3.1 Configure traefik-values.yaml

  * Add below content to traefikcustom-values.yaml file along with the existing content of values-test.yaml.
  * Make sure to clean the resources with certs & related secrets & deploy traefik with these values.
  
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
  # Pebble Configurations:
  - --certificatesresolvers.pebble.acme.tlschallenge=true
  - --certificatesresolvers.pebble.acme.email=test@hello.com
  - --certificatesresolvers.pebble.acme.storage=/data/acme.json
  - --certificatesresolvers.pebble.acme.caserver=<add_the_pebble_acme_url>
  # https://acme-staging-v02.api.letsencrypt.org/directory

volume:
  - name: pebble
    mountPath: /certs
    type: configMap
env:
  - name: LEGO_CA_CERTIFICATES
    value: "/certs/root-cert.pem"

```
### 3.2 Install traefikV2 ssl
 
```sh
$ helm upgrade traefik <path> --install -f <path>/traefikcustom-values.yaml -n kdp
```

### 3.3 Configure Ingressroute for Traefik Dashboard


### 3.4 Configure Ingressroute for Application UI 




