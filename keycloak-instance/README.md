# Pre-requisites:
1. Keycloak operator is deployed and ready
2. A postgres database is ready with a clean database.

For this example: I am using a in-cluster postgres instance.

# Database configuration
The database configuration should be present in the namespace where keycloak instance is creaed. The name of the secret must be keycloak-db-secret. Following values are used for this example:

- POSTGRES_DATABASE=keycloak
- POSTGRES_EXTERNAL_ADDRESS=postgres.keycloak.svc.cluster.local
- POSTGRES_EXTERNAL_PORT=5432
- POSTGRES_PASSWORD=psqlpwd
- POSTGRES_USERNAME=postgres

Please note POSTGRES_EXTERNAL_ADDRESS must be a valid FQDN or IP. ClusterIP for the service doesn't work since internally, keycloak creates a clusterIp service which points to this external database FQDN/IP. As such, virtual IPs are not supported.

# Hostname

Update the hostname in keycloak.yaml based on the dns names you have selected in certificate.

Please note: Keycloak instance doesn't work with subpaths like hostname/subpath. It has to be a valid dns name without subpath.


#logs for debugging

> k logs --follow pod/keycloak-0  > ~/logs/keycloak.txt

# To get admin credentials:
> kubectl get secret credential-sso-keycloak -o go-template='{{range $k,$v := .data}}{{printf "%s: " $k}}{{if not $v}}{{$v}}{{else}}{{$v | base64decode}}{{end}}{{"\n"}}{{end}}'

# If login pasword is not working then, redo a clean install of database.

i.e delete DB,realm, instance and recreate everything. If DB was not cleaned before instance came up, then it will lead to error
