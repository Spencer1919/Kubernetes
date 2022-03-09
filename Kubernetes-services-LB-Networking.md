# [Kubernetes Services, Load Balancing and Networking](https://kubernetes.io/docs/concepts/services-networking/)

## K8s Network Model
- Each Pod get it's it's own IP address
- Pods on a node can communicate with all pods on all nodes without NAT
- agents on a node can communicate with all pods
- containers within a pod share network namespace - IP mac address etc.
- Containers in a pod must coordinate via port usage by IP-Per-pod model
- Kubernetes networking addresses four concerns
  - Containers within a pod use networking to communicate via loopback
  - cluster networking provides communication betwee ndifferent pods
  - Service resource lets' you expose an application runing in pods to be reachable outside of cluster
  - You can use services to publish services only for inside cluster consumption
