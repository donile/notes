# Nodes

Starts up pods

Implement networking

## Kubelet

Monitors API Server on Control Plane Node

Starts and stops pods based upon config changes exposed by API Server

Reports node and pod state

Executes pod probes

## Kube-proxy

Monitors API Server on Control Plane Node

Manages networking

For example, if there is a new pod in the cluster, add it to the network topology

Implements Services

Routes traffic to pods

Load balancing

## Container Runtime

Downloads and runs the containers in pods
