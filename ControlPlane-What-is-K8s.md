# What exactly is the [K8s ControlPlane?](https://kubernetes.io/docs/concepts/overview/components/) 1xx

### What is the ControlPlane?
The control plane makes global decisions about the cluster, detects and responds to cluster events and can be run on any machine in the cluster. It is always trying to go from current state to desired state.

### [kube-apiserver](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)
Component of the control plane(static pod) that exposes K8s API. Think of it as the front-end of the K8s control plane. It validates and configures data for api objects and services REST operations.

### [etcd](https://etcd.io/docs/)
Consistent & highly available key value store used as K8s backing store for ALL cluster data.

### [kube-scheduler]()
Component of the control plane(static pod) that watches for newly created pods with no assigned node and chooses node to run it on.
Factors taken in: individual & collective resource requirements, hardware/software/policy constraints, affinity & anti-affinity, data locality, inter-workload interface and deadlines.

### [kube-controller-manager]()
Control Plane component(static pod) that runs controller processes. Each controller is a separate process but to reduce complexity, they are all compiled into a single bin and run as a single process.
Examples of Controllers:
- Node Controller: Responsible for noticing and responding when nodes go down
- Job Controller: Watches for job objects that represent one-off tasks, then creates Pods for said tasks
- Endpoints Controller: Populates the Endpoints object( that is, joins Services & pods)
- Service Account & token controller: Creates dfault accounts and API access tokens for new namespaces

### [cloud-controller-manager]()
Control Plane compoenent that embeds a cloud-specific(AWS, GCP, Azure etc.) control logic. Essentially, it allows you to link your cluster to your cloud providers API and seperates compoenents that interact with the cloud platform from components that interact with your cluster.
