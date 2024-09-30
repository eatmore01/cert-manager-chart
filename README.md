## INTRO

[How to debug Kubernetes Ingress? (TLS - Cert-Manager - HTTP-01 & DNS-01 Challenges)](https://www.youtube.com/watch?v=DJ2sa49iEKo)

- `example domen`: kube-127-0-0-1.nip.io ( research more for domen like this u can go to [nip.op](https://nip.io/))

#### description

a helm chart for simple setup cert manager with Issuers,ClusterIssuers,Certificates,CertificateRequests,Orders and Challenges


### whts chellenges is the best ?

 dns-01 cause if u needed to trafic ur network requests to another cluster HTTP-01 chellenges can take a few minute to get 
 certificate and this can follow to down time, and withb dns-01 chellenges u can get certificaet before u start route ur trafic

### code sniuppets
---

```yaml
- tls:
  - hosts:
    - domen.com
  - secretName: tls-secretName
```

????
```yaml
annotation:
  cert-manager.io/cluster-issuer: letsencrypt-issuer
```

---

### CERT_MANAGER KUBE INGRESS FLOW

- `CM = CERT MANAGER` 
- 1. CM DETECT tls section in ur ingress
- 2. CM create certificate, create private key and store it in kube secret
- 3. create certificateREquest 
- 4. certificateREquest create order resource
- 5. order resource create a chellenges such as HTTP-01 or DNS-01
- 6. when chelleng is resolved, CA will recive ( получит ) a public certificate and store it in kube secret

### TLS SECURE FLOW

- 1. generate private key and csr ( certificate segning request ) who extends public key.
- 2. send csr to CA ( certificate authority ).
- 3. CA prove its ur domain by chellenges such as HTTP-01 or DNS-01.
- 4. get and return certificate who encrypt with public key for u.
- 5. u storage secur it certificate.

### HTTP-01 chellenges FLOW 

- 1. u say thay u needed certificate for domain
- 2. CA give u secret token and say that can u expose this secret on ur domain with special path
- 3. CA verify this
- 4. CA return certificate

### DNS-01 chellenges FLOW

- 1. u say thay u needed certificate for domain
- 2. CA give u secret token and say that can cretae TXT recodr with this token
- 3. u make it
- 4. CA verify this
- 5. CA return certificate