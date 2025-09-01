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

### Common Transformers

-> commonLabel - adds a label to all Kubernetes resources
-> namePrefix/Suffix - adds a common prefix-suffix to all resource names
-> Namespace - adds a common namespace to all resources
-> commonAnnotations - adds an annotation to all resources

-> commonLabel transformer

just add the commonLabels in kustomization.yaml

```
commonLabels:
    org: KodeKloud
```

-> Namespace transformer

In kustomization.yaml just add the content below

```
namespace: lab
```

-> Name Prefix/Suffix Transformer

```
namePrefix: KodeKloud-

nameSuffix: -dev
```

-> commonAnnotations Transformer

we can simply specify the information below

```
commonAnnotations:
    branch: master
```

### Image Transformers

-> The Images can be tranformed by using the below command

```
images:
    - name: nginx
      newName: haproxy
```

the name is the current name of the image and the newName is the new name that the old name is getting substituted by

-> The tag of the image can be modified by the following

```
images:
    - name: nginx
      newTag: 2.4
```

This will change the tag of the image where no need to specify the old tag but just the name of the current image

-> for example, we wanted to change the new image and the tag, then

```
images:
    - name: nginx
      newName: haproxy
      newTag: 2.4
```


