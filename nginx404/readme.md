# Custom default backend (ie custom error page)
This lets you create a custom default backend, ie if the ingress-nginx controller doesn't 
know where to route a request. This lets you create a custom error page that's a little 
nicer than the default nginx one. You could also implement custom redirect logic etc. by 
editing the nginx config.

### Build the Docker image
Build the `Dockerfile` in this folder and publish to your favourite registry (like [hub.docker.com](hub.docker.com)).
Then update the `default-nginx-backend.yaml` to specify the image you created.

### Create the custom backend container
Apply the `default-nginx-backend.yaml` file to create a container to serve the custom error page with: `kubectl apply -f default-nginx-backend.yaml`

### Install the nginx ingress controller
On most cloud platforms this will automatically provision an externally accessible load balancer.

You can get it's IP address by running this command and looking for the EXTERNAL-IP: `kubectl get svc --namespace=ingress-nginx` 

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