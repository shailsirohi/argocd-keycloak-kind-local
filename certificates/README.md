# Prepare your certificates

Prepare the certificates which are required for enabling ssl/tls.

In this example, we are using self-signed cert with following dns names being supported by the certs:

DNS.1   = argo.127-0-0-1.sslip.io <br>
DNS.2   = 127-0-0-1.sslip.io <br>
DNS.3   = keycloak.127-0-0-1.sslip.io <br>

For using different dns names, argocd ingress and keycloak ingress will need to be updated acordingly
