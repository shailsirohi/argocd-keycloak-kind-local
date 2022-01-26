# About keycloak-operator
Keycloak operator simplifies creation of keycloak instance. However, the operator is not really prod ready. Some of the issues, I encountered:

1) Hardcoded for nginx ingress. If you want to use some other ingress, there are manual interventions required (PS. I did try using Amabassdor ingress but didn't really work seamlessly. So, moved back to nginx. Perhaps, it would work if effort put to debug but there is serious documentation deficit)
2) Realm object shows ready if created declaratively as k8s object. But the realm is not visible when you login into the UI.
3) There is no mention of a clear way on how to provide TLS cert to keycloak instance, if you want to enable ssl passthrough
4) You can't use an existing postgres instance. Everytime a keycloak instance is created, it needs a clean database. If it's an existing database, then the pod just doesn't start and give DB error

# How to run?

> kubectl apply -k .
