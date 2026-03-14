## Network policies

Ingress: flow comming from user to machine
user -> frontend -> backend -> db

Egress: flow going from machine to user
db -> backend -> frontend -> user

CNI - responsible for pod is accessible within the k8s cluster
eg.
weave-net
flannel - don't support network policies
calico
cilium

Network Policies: implement certain rules so that all pods are not connected to each other by default.
