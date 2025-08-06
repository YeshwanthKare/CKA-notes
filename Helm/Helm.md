# Helm

```
helm install wordpress

helm upgrade wordpress

helm rollback wordpress

helm uninstall wordpress
```

## Installing Helm

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

Notes:

1. When a Chat is applied to a cluster, a release is created
    - A release is a single installation of an Application using a Helm chart
    - within each release we can have multiple revisions
    - Each revision is like a snapshot of the application (upgrade of image, change of replicas etc..,)

