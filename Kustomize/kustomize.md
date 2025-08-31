### kustomze vs Helm

-> Helm uses go templates to allow assigning variables to properties
-> Helm is more than just a tool to customize configurations on a per environment basis. Helm is also a package manager for you App
-> Helm provides extra features like conditionals, loops, functions, and hooks
-> Helm templates are not valid YAML as they use go templating syntax
-> Complex templates become hard to read

### Install Kustomize

-> The below command automatically detects the OS and installs the Kustomize

```
curl -s "https://raw.githubusercontent.com/kubernetes-sigs/kustomize/master/hack/install_kustomize.sh"  | bash

kustomize version --short
```

### kustomization.yaml

-> Inside the k8s/ folder the base deployment files to be present
-> along with the deployment files, the kustomization.yaml should also be present which shows the files present in the same folder

Build the deployment using the following command:

```
kustomize build k8s/
```

### kustomize output

The below command does not apply the YAML

```
kustomize build k8s/
```

The below command applies with the kubectl apply

```
kustomize build k8s/ | kubectl apply -f -
```

But if we wanted to apply natively the YAML with the kustomize

```
kubectl apply -k k8s/
```

Now we will see how to delete the YAML

```
kustomize build k8s/ | kubectl delete -f -
```

To delete natively

```
kubectl delete -k k8s/
```

### Kustomize ApiVersion & Kind

can be defined if there are any pattern braking changes in the future

```
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
```

### Managing directories

if we have multiple directories to manage

```
kubectl apply -f k8s/api/

kubectl apply -f k8s/db/
```
