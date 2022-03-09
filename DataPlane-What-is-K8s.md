# What exactly the [K8s DataPlane?](https://kubernetes.io/docs/concepts/overview/components/) 1xx

## Node Components
Node compoenents run on every node, maintaining running pods and providing the K8s runtime environment

### [kubelet]()
Agent that runs on each node in the cluster. It ensures that containers are running in a Pod. Effectively, kubelet takes a set of PodSpecs and ensures that the containers described in the PodSpecs are running and healthy. Kubelet doesn't manage  containers which weren't created by K8s.

### [kube-proxy](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-proxy/)
A network proxy that runs on each node in the cluster, implementing part of the K8s Service Concept.
It maintains network rules on nodes. These specific network rules allow network communication to your pods from network sessions inside or outside of the cluster.
Kube-Proxy also uses the OS packet filtering layer if there is one available, if not, it forwards the traffic itself.
Can do simple TCP, UDP, SCTP stream forwarding, round robin TCP, UDP and SCTP forwarding across a set of backends. Service cluster IP's and ports are currently found through Docker-links-comptaible environment variables specifying ports open by the service proxy.

### [Container runtime](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-node/container-runtime-interface.md)
Software responsible for running containers. This plug-in allows a healthier eco-system that de-couples it from the rest of the environment.

## [Addons](https://kubernetes.io/docs/concepts/cluster-administration/addons/)
Addons utilize k8s resources(DaemonSet, Deployment, replica set etc.) to implement cluster features. Due to them being cluster-level features, namespaces resources for addons belong with the kube-system namespace.

### [DNS](https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/)
All K8s clusters should have cluster DNS, so many things rely on it.
Cluster DNS is a DNS server, in addition to other DNS server(s) in your environment, which serves DNS records for K8s services.

### [Container Resource Monitoring](https://kubernetes.io/docs/tasks/debug-application-cluster/resource-usage-monitoring/)
Records generic time-series metrics about containers in a central db

### [Cluster-level logging](https://kubernetes.io/docs/concepts/cluster-administration/logging/)
Mechanism that is responsible for saving logs to a central log store.


##
