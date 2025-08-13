# To know the cluster init driver

```
ps -p 1
```

# To make a directory containerd

```
sudo mkdir -p /etc/containerd

containerd config default
```

# Initialize Controlplane

```
kubeadm init --apiserver-cert-extra-sans=controlplane --apiserver-advertise-address 192.217.72.10 --pod-network-cidr=172.17.0.0/16 --service-cidr=172.20.0.0/16
```
