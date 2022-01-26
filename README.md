# argocd-keycloak-kind-local
This repository contains code for the argocd and keycloak integration on a local kind cluster.

Each sub directory has its own Readme which explains the action needs to be done for deploying and configuring the component

The sequence to follow is:
1. certificates
2. nginx-ingress
3. postgres
4. keycloak-operator
5. keycloak-instance
6. argocd
7. sso-integration
    7.1 keycloak configuration
    7.2 argocd configuration

All the certs have been removed. So, please make sure the certs are generated and copied inside the appropriate sub-directories inside nginx-ingress and argocd directories. The exact path can be inferred from kustomization.yaml in the aforementioned directories.
