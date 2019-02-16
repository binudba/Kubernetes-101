### Pod Network CNIs 

There are several projects provide K8s pod network using CNIs Calico, Canal, Flannel etc. In this it describes the canal implementation. 
Canal uses Calico for network policy and Flannel for networking. 

#### Why Pod network to be deployed on K8s cluster? 

Kubernetes uses CNIs for pods communication. For Networking and network policy, 3rd party tools like canal or flannel are widely used. With letest version of K8s, CoreDNs is available for service discovery and that provides DNS to pods.
