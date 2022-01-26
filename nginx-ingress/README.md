# Configuring nginx ingress

ssl passthrough has to be enabled for argocd grpc to work. The configuration provided for split ingress in argocd documentation doesn't work. UI login is successfull. However cli login doesn't work

# Certificates

SSL certificates should be provided for tls-secret. Please update kustomization.yaml to point to correct certifacte and private key files

# Enabling ssl passthrough and setting up default tls certificate

1. A tls secret in ingress-nginx needs to be created
2. Following needs to be added in args section of Deployment object of ingress-nginx-controller
  - --default-ssl-certificate=ingress-nginx/tls-secret
  - --enable-ssl-passthrough

# How to run
All the above changes are already made in the files. To deploy nginx ingress, execute:

> cd nginx-ingress <br>
> kubectl apply -k .
