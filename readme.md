# mm-admin
The membermatters.co administration software.

### Setup lets encrypt issuers
Apply the `letsencrypt-staging-issuer.yaml` file to create a cert manager issuer with: `kubectl apply -f letsencrypt-staging-issuer.yaml`.

Apply the `letsencrypt-production-issuer.yaml` file to create a cert manager issuer with: `kubectl apply -f letsencrypt-production-issuer.yaml`.

### Provision a new tenant
Apply the `tenant.yaml` file to create a new tenant with: `kubectl apply -f tenant.yaml`.

This creates the following:
* name space
* web app deployment
* creates a service for the web app deployment
* creates an ingress for the service
* requests and applies a lets encrypt cert for domain(s) specified in ingress

  