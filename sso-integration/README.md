# keycloak configuration

## Create new realm
After loging into the keycloak UI, there will be master with a small dropdown arrow. click on it and add a new realm.

## Role, Group and User Configuration
1. Add a new role. Let's call it argo-admin
2. Create new group. Let's call it system-admin and add argo-admin role to this group
3. Create new user. Put username, first name, last name as admin and press save
4. Now go to credential tab of this new user and add password. Disable temporary so you don't need to reset password. Let's say pwd is creds123 and press set password
5. Now go to groups tab of this user and add it to system-admin group.

## Client Scopes Creation
1. Go to Client Scopes and click on Create
2. In the name field put 'groups' and click on save
3. Go to mappers tab, put name as groups. In the Mapper type, select group membership and token claim name as groups

## Create clients
The documentation only covers configuration required for web sso integration. Argocd cli and web needs different client. As web follows Confidential access-type while cli follows public access-type based on PKCE.

1. Create web client:<br>
    a) Name will be argocd<br>
    b) root url is the url of argocd instance which in this example is https://argo.127-0-0-1.sslip.io and press save<br>
    c) On settings page, change the AccessType to confidential. Redirect url to: https://argo.127-0-0-1.sslip.io/auth/callback and base url as /applications. Click on save.<br>
    d) Go to Client Scopes tab - In the Setup sub tab, groups will be in available client scopes. Add this to default client scopes (PS: It can also be added in optional client scopes)<br>
    e) Go to credentials tab and copy the secret.<br>
    f) Get the base64 encoded version of this secret using:<br>
    > echo '{client secret}' | base64 <br>

    g) Copy the base64 encoded secret and update the argocd-secret-patch.yaml file. Set the value of oidc.keycloak.clientSecret as the base64 encoded secret<br>

2. Create cli client
    a) Create anothe cli client and name it argocdcli and root url as 'https://argo.127-0-0-1.sslip.io'<br>
    b) Keep Access Type to public<br>
    c) Set Redirect URI to http://localhost:8085/auth/callback [This is required because argocd cli opens a tunnel to localhost for sso flow]<br>
    d) Base URL remains as /applications<br>
    e) In advanced settings, set Proof Key for Code Exchange Code Challenge Method as S256 and press save.<br>
    f) Now go to client scopes and add groups<br>

# ArgoCD Configuration

## Update argocd-cm configmap
In argocd-cm-patch.yaml, oidc configuration is suppose to be added. If you made any dns or client name changes, please update it accordingly.

Please note: cliClientId and clientId are different variables

## Update argocd-secret-patch.yaml
It was already updated while creating the keycloak client for argo web.

## Update argocd-rbac-cm-patch.yaml
This configmap is expected to contain all user policies. Currently it is configured to map user-group system admin to argo admin role.


## Configuring the CA certificate
ArgoCD doesn't allow OIDC configuration using self-signed cert. There is a workaround for making the cli work. However, web sso login will still not work.

To make the cli work, while creating argocd objects earlier, the argo-server deployment object has been modifed. The rootCA for our self-signed certificate has been mounted to the pod in directory /etc/ssl/certs/. Look at line #3563 in /argocd/argocd-deploy-2.2.2.yaml in this repo.


## Apply patch to argo objects

-- secret patch
> kubectl patch secret argocd-secret --patch-file argocd-secret-patch.yaml

--configmap server patch
> kubectl patch configmap argocd-cm --patch-file argocd-cm-patch.yaml

-- configmap rbac patch
> kubectl patch configmap argocd-rbac-cm --patch-file argocd-rbac-cm-patch.yaml


# Login using sso through CLI

> argocd login argo.127-0-0-1.sslip.io --sso

Use the credential created for user while configuring keycloak.
