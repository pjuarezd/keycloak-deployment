# Install basic keycloak in kubernetes

## (Optional) Certificates
Create a private key and a public certificate for the keycloak service for the domain `kc-service.svc`, add this certificate to the secret in `keycloak-certificate-secret.yaml`, the example includes a certificate and private key that can be used as a example.

## Apply resources

Apply kustomization will create Keycloak CRD, Keycloak Operator, and a Keycloak resource, as well a postgres database deployment, along with the services to access each one.

```sh
kubectl apply -k .
```

## Get keycloak admin user and password

Username

```sh
kubectl get secret -n keycloak kc-initial-admin -oyaml | yq '.data.username' | base64 --decode
```

Password:

```sh
kubectl get secret -n keycloak kc-initial-admin -oyaml | yq '.data.password' | base64 --decode
```

## Access keycloak inside and outside k8s cluster

You will need access to the keycloak service from inside the kubernetes cluster and from outside of it in your local machine to run the authentication flow, with kubefwd create a port forward and DNS in your local dev machine to access the services in the keycloak namespace as if they were hosted locally.

```sh
sudo kubefwd svc -n keycloak
```

Open keycloak https://kc-service.keycloak.svc.cluster.local:8443/