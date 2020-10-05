# Custom default backend (ie custom error page)

### Create the custom backend container
Apply the default-nginx.yaml file to create a container to serve the custom error page with: `kubectl apply -f default-nginx-backend.yaml`

### Install the nginx ingress controller
On most cloud platforms with will automatically provision an externally accessible load balancer.

Install the nginx ingress controller with: `helm install ingress-nginx --namespace ingress-nginx ingress-nginx/ingress-nginx`

### Edit the config to specify custom backend container
Then run `kubectl edit deployment/ingress-nginx-controller -n ingress-nginx` and edit/add a line specifying the `default-backend-service` so it looks like below:
```
apiVersion: apps/v1
kind: Deployment
[...]
spec:
  [...]
  template:
    spec:
      containers:
      - args:
        - /nginx-ingress-controller
        - --configmap=$(POD_NAMESPACE)/nginx-configuration
        - --tcp-services-configmap=$(POD_NAMESPACE)/tcp-services
        - --udp-services-configmap=$(POD_NAMESPACE)/udp-services
        - --publish-service=$(POD_NAMESPACE)/ingress-nginx
        - --annotations-prefix=nginx.ingress.kubernetes.io
        - --default-backend-service=$(POD_NAMESPACE)/custom-default-backend
[...]

``` 