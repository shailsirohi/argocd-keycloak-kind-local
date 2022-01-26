The example is using k8s 1.22 version.

# create kind cluster with 1.22

> kind create cluster --config kind.yaml --image kindest/node:v1.22.0@sha256:b8bda84bb3a190e6e028b1760d277454a72267a5454b57db34437c34a588d047

# Ports and Local storage
All the ports and storage mount configuration is in kind-ingress-storage.yaml

Please note if you are on Mac or Windows and using docker desktop, you have to update docker settings to allow file sharing on hostPath in extraMounts section in the kind cluster config file.

This example is done on ubuntu laptop. There is no docker configuration requied on ubuntu.
The host path in this example: /home/kind/mgmt/sharedstorage. This is where the postgres instance data will be stored/

PS. Postgres is required by keycloak as external database. The embedded DB configuration has been deprecated by keycloak
