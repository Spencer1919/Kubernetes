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

### [Service](https://kubernetes.io/docs/concepts/services-networking/service/)
An abstract way to expose an application running on a set of pods as a network service. K8s gives Pods their own IP addresses and a single DNS name for a set of Pods, and can load-balance across them. Problem: how do you keep track of backend(pods) to the frontend(pods) and which IP's to use. Enter Services

- A service in K8s is a REST objects; like all REST objects, you can POST a service  definition to the API server to create a new instance.
  - A service can map any incoming port to a target port.
  - In the example below, suppose you have a set of pods that listen to port 9376 and have the label app=MyApp. This service targets any port that meets the below specs.

'''

apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376

'''

- Services without selectors:
  - You'd like an external dab cluster in production but you use your own db's in a test environment
  - You want to point your service to a service in a different NameSpace or cluster
  - Migration a workload to K8s, while evaluating the approach, you run only a portion of your backends in K8s.

### [ServiceTypes](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types)
There are various Service types out there that allow you to specify which you would prefer.
- Cluster IP: Exposes the Service on a cluster-internalIP. Readable only in cluster.
- NodePort: Exposes the Service on each Node's IP on a static port. ClusterIP service, to which the NodePort Service routes. You can connect to this outside the cluster.
- LoadBalancer: Exposes the service externally using a cloud providers LB. NodePort and ClusterIP services, to which the external LB routes, are auto created.
- ExternalName: Maps the service to the content of the externalNmae field by returning a CNAME record with it's value. No proxying of any kind is setup.
- Ingress: you can use Ingress to expose your Service though it's not a service type. It's an entry point for your cluster, consider routing rules and exposes multiple services via single IP.

#### LoadBalancer Example
Traffic from external load balancer is directed towards backend pods. Cloud Provider dictates how it's load balanced. 
'''
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
  clusterIP: 10.0.171.239
  type: LoadBalancer
status:
  loadBalancer:
    ingress:
    - ip: 192.0.2.127
'''
