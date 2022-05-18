# Code Engine custom domain with Cloud Foundry or Kubernetes service and Traefik
For some of my apps deployed on IBM Cloud Code Engine I am requiring a custom domain. It seems a feature is in the making. Until then, I use Traefik (https://traefik.io/traefik/) deployed to IBM Cloud with Public Cloud Foundry or the Kubernetes service as workaround. It only requires few simple steps.

## Instructions for Kubernetes service (WIP)

The following assumes that you have a Standard cluster provisioned in the IBM Cloud Kubernetes service (IKS). For my tests and deployment, I have used the VPC-based clusters.

0. Have a Standard cluster ready. Be logged in on the CLI and ready to use `kubectl`.
1. Create a CNAME entry for your (wildcard) subdomain to point to the Ingress subdomain of your Kubernetes cluster. Use the following command to find the Ingress subdomain. See [](https://cloud.ibm.com/docs/containers?topic=containers-ingress-types#alb-com-setup-domain) for additional help.
   ```
   ibmcloud ks cluster get --cluster your-cluster-name
   ```
2. Adapt the routes.yml to your routing requirements. Then, create a configMap with that file:
   ```
   kubectl create configmap cm-traefik-routes --from-file=routes.yaml=routes.yaml
   ```
   The file is later made available to the running traefik proxy. The configMap is referenced in [traefik-deploy-simple.yaml](traefik-deploy-simple.yaml)
3. If not done already, generate the TLS certificates for your custom domain. I used Let's Encrypt and created wildcard certificates for (a subdomainof ) my domain. Next, make the certificate and key available as Kubernetes secret:
   ```
    kubectl create secret tls tls-custom-domain --cert=certificates/_.example.com.crt --key=certificates/_.example.com.key
   ```
   The secret is referenced in the deployment configuration, too.
4. Adapt the deployment configuration in [traefik-deploy-simple.yaml](traefik-deploy-simple.yaml) to your needs. Then, deploy it:
   ```
   kubectl apply -f traefik-deploy-simple.yaml
   ```   

Thereafter, your app on Code Engine should be accessible using the custom domain.

To update the deployment, edit the configuration file and run the `apply` again:
```
kubectl apply -f traefik-deploy-simple.yaml
```   

To update the routing, edit `routes.yaml`, then update the configMap in place:
```
kubectl create configmap cm-traefik-routes --from-file=routes.yaml=routes.yaml -o yaml   --dry-run=client | kubectl apply -f -
```   

