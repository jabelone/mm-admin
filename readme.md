# mm-admin
The membermatters.co administration software.

### Setup lets encrypt issuers (optional)
**Note:** remove the `cert-manager.io/cluster-issuer` annotation from the Ingress in `tenant.yaml` if you want to skip SSL 
certificates for testing. You *must* use a domain mapped to the public IP address of your pod's load balancer to get a 
valid SSL certificate.

This will allow you to generate valid Let's Encrypt certificates for every domain you add
to the k8s cluster. You can change which one the k8s deployment uses for each tenant by changing `cert-manager.io/cluster-issuer:` in `tenant.yaml` to `"letsencrypt-staging"` or `"letsencrypt-production"`.

#### Lets Encrypt Staging Server
This lets you use the Let's Encrypt staging server for testing as the prod server is heavily rate limited. 
Use this to test/configure stuff then use the production one when ready.

Apply the `letsencrypt-staging-issuer.yaml` file to create a cert manager issuer with: `kubectl apply -f letsencrypt-staging-issuer.yaml`.

#### Let's Encrypt Production Server
You should use this in production to issue valid certificates.

Apply the `letsencrypt-production-issuer.yaml` file to create a cert manager issuer with: `kubectl apply -f letsencrypt-production-issuer.yaml`.

### Provision a new tenant
Apply the `tenant.yaml` file to create a new tenant with: `kubectl apply -f tenant.yaml`.

This creates the following:
* name space
* web app deployment
* creates a service for the web app deployment
* creates an ingress for the service
* requests and applies a lets encrypt cert for domain(s) specified in ingress

  