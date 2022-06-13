# Deploy-Traefik-Ingress-Controller-on-kubernetes-using-YAMLs
## Ingress Controller
While exposing your application as a service to make it avalable for the external access, the Service resource defined as part of K8s networking plays an important role. Services that get associated with underlying Pods through selectors and allows them to be accessed depends upon service type configured.

In K8s we can define a native Ingress resource abstraction which exposes HTTP and HTTPS endpoints and routes traffic based upon rules defined by the user. Ingress Controller provides SSL termination, load balancing and name-based virtual hosting. It simplifies the interaction of internal services and can re-route by only changing Routing Rules. Allows external https traffic, terminating encryption and allow the http traffic between services within cluster. LoadBalances traffic between services hosted outside the Kubernetes cluster.

### Deploy Traefik using Helm chart
Create a traefik namespace. Add the Helm repository hosting the Traefik charts metadata. Update local Helm Chart repository chache. Search for latest traefik/traefik official Helm chart version. Install the latest Traefik Helm chart and finally Verify installation.

```
kubectl create namespace traefik
```
```
helm repo add traefik https://helm.traefik.io/traefik 
```
```
helm repo update
```
```
helm search repo traefik 
```
``` 
helm upgrade --install traefik --namespace traefik --set dashboard.enabled=true --set rbac.enabled=true --set="additionalArguments={--api.dashboard=true,--log.level=INFO,--providers.kubernetesingress.ingressclass=traefik-internal,--serversTransport.insecureSkipVerify=true}" traefik/traefik --version 10.19.4
```

## IngressRoute and Middleware

Now in order to access the dashboard we have created an IngressRoute and Middleware yamls. IngressRoute is for the domain name on which we will access the dashboard and middleware is for basic authentication, which we will configure. Make sure to changeyour domain name accordingly.

## Authentication

Lets create a user / password basic authentication as shown below. --from-file is the path to the folder containing the file.
```
kubectl create secret generic traefik-dashboard-auth-secret --from-file=$HOME/temp/traefik-ui-creds/htpasswd --namespace traefik
```
