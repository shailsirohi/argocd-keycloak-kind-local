# All postgres files are based on blog by Sumologic for a simle psql Installation
https://www.sumologic.com/blog/kubernetes-deploy-postgres/

# To run:
> cd postgres <br>
> kubectl apply -k .

Please note the database objects will be created in keycloak database

# For testing inside kubernetes cluster,
 > kubectl apply -f psql-test-pod.yaml <br>
 > kubectl exec -it psql-client -- sh <br>
 > psql -h postgres -p 5432 --Username postgres --password <br>
 > password is: psqlpwd

# For testing on you local machine:
 > psql -h localhost -p 30000 -U postgres --password

# Create keycloak database
 > create database keycloak;

# Validate if database has been created
> \l
