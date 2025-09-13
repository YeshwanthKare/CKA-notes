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

### Patches

-> Kustomize patches provide another method to modifying kubernetes configs
-> Unlike common transformers, patches provide a more "surgical" approach to targeting one or more specific sections in a Kubernetes resource
-> To create a patch 3 parameters must be provided: - Operation Type: add/remove/replace - Target: What resource should this patch be applied on
_ Kind
_ Version/Group
_ Name
_ Namespace
_ labelSelector
_ AnnotationSelector - Value: What is the value that will either be replaced or added with (only needed for add/replace operations)

-> Replace operation with Patches

```
patches:
    - target:
        kind: Deployment
        name: api-deployment

      patch: |-
        - op: replace
          path: /metadata/name
          value: web-deployment

---
# this is a Json 6902 Patch

patches:
    - target:
        kind: Deployment
        name: api-deployment

      patch: |-
        - op: replace
          path: /spec/replicas
          value: 5
```

-> In kustomize, we have two different kinds of patches

1. Json 6902 Patch
2. Strategic Merge Patch

```
# Strategic merge patch

patches:
    - patch: |-
        apiVersion: apps/v1
        kind: Deployment
        metadata:
            name: api-deployment
        spec:
            replicas: 5
```

### Different types of Patches

-> Inline for Json 6902 patch

```
Kustomization

patches:
    - target:
        kind: Deployment
        name: api-deployment

      patch: |-
        - op: replace
          path: /spec/replicas
          value: 5
```

-> Separate File for Json 6902 patch

```
Kustomization

patches:
    - path: replica-patch.yaml
      target:
        kind: Deployment
        name: nginx-deployment


replica-patch.yaml

- op: replace
  path: /spec/replicas
  value: 5
```

-> Inline for Strategic merge patch

```
patches:
    - patch: |-
        apiVersion: apps/v1
        kind: Deployment
        metadata:
            name: api-deployment
        spec:
            replicas: 5
```

-> Separate file for Strategic merge patch

```
Kustomization

patches:
    - replica-patch.yaml


replica-patch.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
    name: api-deployment
spec:
    replicas: 5
```

### Patches Dictionary

-> Replace Dictionary in Json6902

```
Kustomization

patches:
    - target:
        kind: Deployment
        name: api-deployment

      patch: |-
        - op: replace
          path: /spec/template/metadata/labels/component
          value: web
```

-> Replace Dictionary Strategic Merge Patch

```
Kustomization

patches:
    - label-patch.yaml

label-patch.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
    name: api-deployment
spec:
    template:
        metadata:
            labels:
                component: web

```

-> Add Dictionary in Json6902

```
Kustomization

patches:
    - target:
        kind: Deployment
        name: api-deployment
      patch: |-
        - op: add
          path: /spec/template/metadata/labels/org
          value: KodeKloud
```

-> Add Dictionary in Strategic Merge Patch

```
Kustomization

patches:
    - label-patch.yaml

label-patch.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
    name: api-deployment
spec:
    template:
        metadata:
            labels:
                org: kodekloud
```

-> Remove Dictionary Json6902

```
Kustomization

patches:
    - target:
        kind: Deployment
        name: api-deployment
      patch: |-
        - op: remove
          path: /spec/template/metadata/labels/org
```

-> Remove Dictionary in Strategic Merge Patch

```
Kustomization

patches:
    - label-patch.yaml


label-patch.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
    name: api-deployment
spec:
    template:
        metadata:
            labels:
                org: null
```

### Patches List

-> Replace List in Json6902

```
Kustomization

patches:
    - target:
        kind: Deployment
        name: api-deployment
      patch: |-
        - op: replace
          path: /spec/template/spec/containers/0
          value:
            name: haproxy
            image: haproxy
```

-> Replace List Strategic Merge Patch

```
Kustomization

patches:
    - label-patch.yaml


label-patch.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
    name: api-deployment
spec:
    template:
        spec:
            containers:
                - name: nginx
                  image: haproxy
```

-> Add List Json6902

```
Kustomization

patches:
    - target:
        kind: Deployment
        name: api-deployment
      patch: |-
        - op: add
          path: /spec/template/spec/containers/-
          value:
            name: haproxy
            image: haproxy
```

-> The - signifies that it will add the new container at the end of the list, i.e., it will append

-> Add List Strategic Merge Patch

```
Kustomization

patches:
    - label-patch.yaml


label-patch.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
    name: api-deployment
spec:
    template:
        spec:
            containers:
                - name: haproxy
                  image: haproxy
```

-> Delete List Json6902

```
Kustomization

patches:
    - target:
        kind: Deployment
        name: api-deployment
      patch: |-
        - op: remove
          path: /spec/template/spec/containers/1
```

-> Delete List Strategic Merge Patch

```
Kustomization

patches:
    - label-patch.yaml


label-patch.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
    name: api-deployment
spec:
    template:
        spec:
            containers:
                - $patch: delete
                  name: database
```

### Overlays

-> Base/kustomization

```
resources:
    - nginx-depl.yaml
    - service.yaml
    - redis-depl.yaml
```

-> Base/nginx-depl.yaml

```
apiVersion: apps/v1
kind: Deployment
metadata:
    name: nginx-deployment
spec:
    replicas: 1
```

-> dev/kustomization

```
bases:
    - ../../base
patch: |-
      - op: replace
        path: /spec/replicas
        values: 2
```

-> prod/kustomization

```
bases:
    - ../../base
patch: |-
      - op: replace
        path: /spec/replicas
        value: 3
```

#### Adding new files in obverlays

-> prod/kustomization

```
bases:
    - ../../base

resources:
    - grafana-depl.yaml

patch: |-
      - op: replace
        path: /spec/replicas
        value: 2
```

### Components

-> Components provide the ability to define reusable pieces of configuration logic (resources + patches) that can be included in multiple overlays
-> Components are useful in situations where applications support multiple optional features that need to be enabled only in a subset of overlays
