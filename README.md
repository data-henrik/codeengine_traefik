# Code Engine custom domain with Cloud Foundry or Kubernetes service and Traefik
For some of my apps deployed on IBM Cloud Code Engine I am requiring a custom domain. It seems a feature is in the making. Until then, I use Traefik (https://traefik.io/traefik/) deployed to IBM Cloud with Public Cloud Foundry or the Kubernetes service as workaround. It only requires few simple steps.

## Instructions for Kubernetes service (WIP)

The following assumes that you have a Standard cluster provisioned in the IBM Cloud Kubernetes service (IKS). For my tests and deployment, I have used the VPC-based clusters.
