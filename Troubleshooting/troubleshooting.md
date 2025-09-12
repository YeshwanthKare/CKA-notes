### Troubleshooting the controlplane

```
kubectl get nodes

kubectl get pods
```

### If controlplane components were deployed with Kubeadm tool 

```
kubectl get pods -n kube-system
```

### If controlplane components are deployed as services

```
service kube-apiserver status

service kube-controller-manager status

service kube-scheduler status

service kubelet status

service-kube-proxy status
```

### Check service logs

```
kubectl logs kube-apiserver-master -n kube-system
```

-> If service deployed in master

```
sudo journalctl -u kube-apiserver
```

### YAML Manifests 

-> YAML manifests of kube-scheduler 

```
sudo vi /etc/kubernetes/manifests/kube-scheduler.yaml
```

-> YAML certificates for K8s

```
ls /etc/kubernetes/pki
```

### Worker Node Failure

-> Check Node Status

```
kubectl get nodes
```

-> If the Status is "NotReady", check the details 

```
kubectl describe node worker-1
```

-> When a Node is out of Disk space then the out of disk is set to true
-> When the node is out of memory then the memory flag is set to true
-> When the disk capacity is low, then the disk pressure flag is set to true
-> When there are too many processes then the PID pressure is set to true
-> If the node is finally healthy then the "Ready" flag is set to true
-> When the worker node is stop communicating with the master, maybe due to crash then the status is set to Unknown,
this can indicate the possible loss of the node
-> Then check the "LastHeartbeatTime" for the time when the node might have crashed, in such cases proceed to check the status of node itself, like if the node is online at all or it is crashed, if it is crashed bring it back up

-> Check for possible CPU, Memory etc

```
top

df -h
```

-> Check Kubelet Status

```
service kubelet status
```

-> Check the kubelet logs for possible issues

```
sudo journalctl -u kubelet
```

-> Check the status of certificates, if they are expired or the certificates are issued by the right CA

```
openssl x509 -in /var/lib/kubelet/worker-1.crt -text
```
