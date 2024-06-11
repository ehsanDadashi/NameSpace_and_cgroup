# NameSpace_and_cgroup
how to create a container in linux without install container manager tools

# first create two name space 
ip netns add NS1

ip netns add NS2

# see list of name spaces in your linux server
ip netns list

![image](https://github.com/ehsanDadashi/NameSpace_and_cgroup/assets/29996315/6e3dbd65-0d22-4ab6-a4ac-2bf607be8218)

# create virtual eth for namespaces and link it
ip link add veth_ns1 type veth peer name veth_ns2

# asgin veth to name spaces
ip link set veth_ns1  netns NS1

ip link set veth_ns2  netns NS2

# set ip for namespaces NS1 and up the veth
ip netns exec NS1 /bin/bash

ip addr add 10.20.20.1/24 dev veth_ns1

ip link set veth_ns1 up

# set ip for namespaces NS2 and up the veth
ip netns exec NS2 /bin/bash

ip addr add 10.20.20.2/24 dev veth_ns2

ip link set veth_ns2 up

# start port 80 in name space 1
ip netns exec NS1 /bin/bash

ip netns exec NS1 python3 -m http.server 80 &

# start port 80 in name space 2
ip netns exec NS2 /bin/bash

ip netns exec NS2 python3 -m http.server 80 &

# login to NS1 and check NS2's 80 port 
ip netns exec NS1 /bin/bash

nc -zv 10.20.20.2 80

![image](https://github.com/ehsanDadashi/NameSpace_and_cgroup/assets/29996315/6d8ad1d8-1682-4487-a07b-3103c9284fbf)

# shape of our sample
![image](https://github.com/ehsanDadashi/NameSpace_and_cgroup/assets/29996315/9d427d86-a540-4422-899c-f15f2c289c4f)

