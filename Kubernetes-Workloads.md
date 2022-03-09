# Kubernetes Workload
A workload is an application running on Kubernetes, whether a single component or several that work together. The workload is run inside a set of pod(s).


## [Pods](https://kubernetes.io/docs/concepts/workloads/pods/)
Pods are the smallest deployable units of computing that you can create/manage in K8s. It's a group of one or more containers with shared storage, network resources and a specification for how to run with conatiners.

- Pods can run a single container
- Pods can run multiple containers that need to work together.
  - Example: one container serving  data stored in a shared volume while a sidecar container  refreshes or updates those files.
- Pods have init containers and app containers as well.
  - init containers run and complete before the app containers are started.
- Static Pods - managed directly by  the kubelet daemon on a specific node without API server observing them. They are bound to the kubelet and main use is to run a self-hosted control plane; hence, why most control plane components are static pods.
- Container Probes = diagnostic performed periodically by the kubelet on ta container.

### [Pod Lifecycle](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/)
Pods are considered ephemeral(not durable entities) They do not self heal, K8s uses controllers to handle the work of managing the disposable pod instances.




## [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)
Replicaset's core purpose is to maintain a stable set of replica pods running at any given time.

## [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
A deployment provides declarive updates for Pods and ReplicaSets.
You describe a desired state in the spec and the deployment controller changes actual state to desired state at a controlled rate you set.

### Use Case
- Create a deployment to rollout a replicaset
- Declare the new state of pods
- Rollback to earlier deployment
- scale Deployment
- pause the deployment
- pause the rollout of deployment
- clean up older replica sets

## [StatefulSet](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)
Let's you run one or more related pods that do track state. Example: If you're workload records data persistently, you run a Stateful set that matches each pod with a Persistent Volume. Your code running in the pods in the Stateful set can replicate data to other pods in the same StatefulSet to improve overall resilience.

### Use Case(s)
- Stable, unique network identifiers
- Stable, persistent storage
- Ordered, graceful deployment and scaling
- Ordered, automated rolling updates

### Limitations
- Storage for pod must be provisioned
- Deleting and/or scaling a stateful set will not delete the volumes
- [Require Headless Service](https://kubernetes.io/docs/concepts/services-networking/service/#headless-services)
- Do not provide guarantees on the termination of pods when a StatefulSet is deleted
- When using rolling updates with default PMP, it's possible you have to manually terpair it.

## [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)
Defines Pods that provide node-local facilities. Might be fundamental to cluster opeartion such as a networking helper or add-on. Every time you add a node to your cluster that matches the DaemonSet specification, the control plane schedules a pod for that Demonset on said node.


## [Job](https://kubernetes.io/docs/concepts/workloads/controllers/job/) and [CronJob](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)
Define tasks that run to completion and then stop. Jobs represent one-off tasks, whereas CronJobs recur according to a schedule


## [Custom Resource definitions](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)
Custom resources are extensions of the K8s API. It's not necessarily available in a default K8s extension and represents a customization of K8s installation.
