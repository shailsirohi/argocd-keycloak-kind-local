# Generate initial admin password
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo

# argocli login
argocd login argo.127-0-0-1.sslip.io --insecure --username admin --grpc-web-root-path /

# Update password
argocd account update-password


https://stackoverflow.com/questions/63820564/kubernetes-how-to-subdomain-localhost-using-nginx-ingress-controller


The split nginx for  grpc and web just doesn't work.
