# Code Engine custom domain with Cloud Foundry and Traefik
For some of my apps deployed on IBM Cloud Code Engine I am requiring a custom domain. It seems a feature in the making. Until then, I use Traefik (https://traefik.io/traefik/) deployed to IBM Cloud with Public Cloud Foundry as workaround. It only requires few simple steps.

## Instructions

0. Make sure your custom domain is available to Cloud Foundry on IBM Cloud: https://cloud.ibm.com/account/cloud-foundry/. Upload SSL/TLS certificates.
1. Download or clone this repo
2. Download the Traefik Linux AMD64 binary and place it into the directory. See https://github.com/traefik/traefik/releases for the latest release.
3. Adapt routes.yml to your Code Engine app URI and your custom domain. ([Traefik route configuration](https://doc.traefik.io/traefik/routing/overview/))
4. Adapt manifest.yml to your custom domain. ([Cloud Foundry routes](https://docs.cloudfoundry.org/devguide/deploy-apps/manifest.html))
5. Login to IBM Cloud, set the endpoints (ibmcloud target --cf)
6. Push the app: ibmcloud cf push

Once deployed, the configured URI should serve your Code Engine app.

### Path-based routing
See the branch [path-based-routing](https://github.com/data-henrik/codeengine_traefik/tree/path-based-routing) for information on how to map incoming requests to separate Code Engine apps based on the path prefix.
