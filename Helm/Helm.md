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

# chart.yml

1. In a chart.yaml the version is corressponding to the Helm version, if the version is v1 or no version, then it is Helm version 2
2. If the version is v2 then it is Helm version 3

-> To Search for a Helm repo

```
helm search repo
```

-> To search for Helm type and version

```
helm search repo

helm search hub repo(wordpress) / helm search repo wordpress

helm search repo <reponame>/<chartname> --versions
```

-> To add the helm repo

```
helm repo add <reponame> <repo-link>
```

-> To Install the Helm chart

```
helm install my-release bitnami/wordpress
```

-> To list the releases of helm chart

```
helm list
```

-> To uninstall release

```
helm uninstall my-release
```
