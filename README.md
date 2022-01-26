# argocd-keycloak-kind-local
This repository contains code for the argocd and keycloak integration on a local kind cluster.

Each sub directory has its own Readme which explains the action needs to be done for deploying and configuring the component

The sequence to follow is:
1. certificates
2. kind-cluster
3. nginx-ingress
4. postgres
5. keycloak-operator
6. keycloak-instance
7. argocd
8. sso-integration<br>
    8.1 keycloak configuration<br>
    8.2 argocd configuration<br>

All the certs have been removed. So, please make sure the certs are generated and copied inside the appropriate sub-directories inside nginx-ingress and argocd directories. The exact path can be inferred from kustomization.yaml in the aforementioned directories.
