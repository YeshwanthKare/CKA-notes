Docker Networking
=================

1. None :- docker run --network none nginx
2. Host :- docker run --network host nginx (two containers cannot run on the same port at the same time)
3. Bridge:- 

    - ip link add docker0 type bridge
    - docker network ls
    - ip link
    - ip addr
    - docker run nginx
    - ip netns
    - docker inspect 942d70e585b2

-> how to link the containers to the bridge
    - ip link
    - ip -n 942d70e585b2 link

CNI (container network Interface)
=================================

-> Container Runtime must create network namespace
-> Identify network the container must match to
-> Container Runtime to invoke Network Plugin (bridge) when container is ADDed
-> Container Runtime to invoke Plugin (bridge) when container is DELeted.
-> JSON format of the Network Configuration

Plugin: (Bridge, VLAN, IPVLAN, MACVLAN, WINDOWS) and DHCP, host-local 
-> Must support command line arguments ADD/DEL/CHECK
-> Must support parameters container id, network ns etc...
-> Must manage IP Address assignment to PODs
-> Must Return results in a specific format

other third party plugins: Weave, flannel, cilium, VMwareNSX, Calico, infobox etc

for adding the plugin to docker 
===============================
-> docker run --network=none nginx
-> bridge add 2e34dcf34 /var/run/netns/2e34dcf34


To find out which had more network connectivity of ETCD:
=======================================================

netstat -npa | grep -i etcd | grep -i 2380 | wc -l



