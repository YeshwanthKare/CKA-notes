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
