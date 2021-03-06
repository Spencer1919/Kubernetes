# What exactly is a container!

### What is a container?
Lightweight packages of application code together bound together with dependencies such as specific version of programming language runtimes and libraries required to run your software services.

### What's the benefits of containers?
Seperation of responsibility, workload portability and application isolation(virtualize CPU, memory, storage and network resources)

### What are containers made of?
Containers are made up of Cgroups and namespaces

### What are Cgroups and namespaces?
There are [great resources](https://jvns.ca/blog/2016/10/10/what-even-is-a-container/) out there on this but let me summarize below

  - Namespaces = process's should be separated from other process.
    - PID namespace - Become PID1 and then children are your processes
    - Networking namespace -> You can run programs on any port you want without conflicting what's already there.
    - Mount Namespace -> mount and unmount filesystems without affecting host filesystem

  - Cgroups (resource limits) = Limiting CPU, networking bandwidth, IO or memory of what one of your programs are you using

### What is a [Container Image?](reference: https://kubernetes.io/docs/concepts/containers/)
Ready-to-run software package containing everything needed to run an application: code and any runtime it requires, application and system libraries, and default values for any essential settings. By design, it's immutable and a new image must be built.

### What is a [Container runtime?](https://kubernetes.io/docs/setup/production-environment/container-runtimes/)
Software that is responsible for running a containers.


### What is [Container Runtime interface?](https://kubernetes.io/blog/2016/12/container-runtime-interface-cri-in-kubernetes/)

1. Kubelet (grpc client)<-> CRI SHIM(grpc server) <-> container runtime ->>> container(s)
  - Kubelet communicates with the container runtime over unix sockets using the gRPC framework, where kubelet acts as a client and the CRI shim as the server.
  - ImageService provides RPC's to pull in an image from a repository, inspect, and remove an image.
  - Runtimeservice containers RPC's to manage the lifecycle of the pods, containers and interact with containers(exec/attach/port-forward)
  - PodSandbox -> Before starting a pod, kubelet calls RuntimeService to create the environment. This includes setting up networking for a pod(allocating IP). Once PodSandbox is active, indvidual containers can be created/started/stopped/removed indepdently.
  - To delete the pod, kubelet will stop and remove containers before stopping and removing the PodSandbox.
  - Kubelet is responsible for managing the lifecycles of the containers through the RPC's, exercising the container lifecycles hooks and liveness/readiness/startup checks.

### How to [create a container?](https://medium.com/@arpitkh96/basics-of-container-networking-with-linux-part-1-3a3cdc64c87a)

  ```
  #/bin/bash
  set -x
  # Add a namespace named con1
  ip netns add con1
  # Add a veth pair
  ip link add veth11 type veth peer name veth10
  # Assign one of the pair to con1
  ip link set veth10 netns con1
  # Add an ip address (notice the /24 subnet)
  ip netns exec con1 ip addr add 10.0.0.2/24 dev veth10
  # Make both the devices up
  ip netns exec con1 ip link set dev veth10 up
  ip link set dev veth11 up
  # Create loopback interface
  ip netns exec con1 ip link set lo up
  # Add default route to the gateway
  ip netns exec con1 ip route add default via 10.0.0.1 dev veth10

  # Add a namespace named con2
  ip netns add con2
  # Add another veth pair
  ip link add veth21 type veth peer name veth20
  # Assign one of the pair to con2
  ip link set veth20 netns con2
  # Add an ip address (notice the /24 subnet)
  ip netns exec con2 ip addr add 10.0.0.3/24 dev veth20
  # Make both the devices up
  ip netns exec con2 ip link set dev veth20 up
  # Create loopback interface
  ip link set dev veth21 up
  ip netns exec con2 ip link set lo up
  # Add default route to the gateway
  ip netns exec con2 ip route add default via 10.0.0.1 dev veth20

  # Add a bridge named br0
  ip link add name br0 type bridge
  # Attach the veth pair to bridge
  ip link set dev veth11 master br0
  ip link set dev veth21 master br0
  # Add an IP to the bridge
  ip addr add 10.0.0.1/24 dev br0
  ip link set dev br0 up
  ```

## Docker basics
### What is Docker?
Open source containerizaton platform

### [Cheat Sheet how to Docker](https://github.com/wsargent/docker-cheat-sheet#prerequisites)
- Build:
  - docker build -t myimage:1.0
  - Docker image ls
  - Docker image rm alpine:3.4
- Run:
  - docker container run --name web -p 5000:80 alpine:3.9
  - docker container stop web
  - docker container kill web
  - docker network ls
  - docker container logs --tail 100 web
