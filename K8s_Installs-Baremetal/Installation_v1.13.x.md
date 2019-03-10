
#### Kubernetes Installation v1.13.2 - Single Master

Following are the basic pre-installation and installaion steps for K8s v1.13.x installation. Each server should have minimum of 2GB memory and 2 CPUs.

##### Pre-validations

  1. Check firwalld stauts and disable firewalld.
  ```
      systemctl stop firewalld
      systemctl disable firewalld
      systemctl status firewalld
  ```    
  2. Disable SELinux
  
      Edit following files and verifiy SELINUX parameter is set to disabled.
 ```     
      sed -i 's/SELINUX=permissive/SELINUX=disabled/g' /etc/sysconfig/selinux
      sed -i 's/SELINUX=permissive/SELINUX=disabled/g' /etc/selinux/config
 ```     
  3.   Disable swap
  
  ```
        swapoff -a
        
  ```

##### Docker installation - version 18.06

  1. Setup the repo

```
    yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    yum install docker-ce-18.06.1.ce-3.el7.x86_64

  	systemctl enable docker
	  systemctl start docker
```
      
##### K8s Installation - Master node

  1. Setup K8s repo
  
    cat <<EOF > /etc/yum.repos.d/kubernetes.repo
    [kubernetes]
    name=Kubernetes
    baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
    enabled=1
    gpgcheck=1
    repo_gpgcheck=1
    gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
    exclude=kube*
    EOF

  2. Config settings for iptables on CentOS or RHEL
```  
  cat <<__EOF__ >  /etc/sysctl.d/k8s.conf
  net.bridge.bridge-nf-call-ip6tables = 1
  net.bridge.bridge-nf-call-iptables = 1
  __EOF__

  sysctl --system
  sysctl -p
  modprobe br_netfilter
```  
  
 3. Install the latest version of K8s
 
```
    yum install kubeadm kubelet kubectl
```

  4. Configure cgroup driver for kubelet

```
    docker info | grep -i cgroup
    
```
    edit /var/lib/kubelet/kubeadm-flags.env and verify the following : cgroup-driver=cgroupfs 

```    
    KUBELET_KUBEADM_ARGS=--cgroup-driver=cgroupfs
```

  5. Bootstraping Kubernetes cluster with kubeadm - single node 
  
     Initializing K8s master and select netwrok addon (example: flannel or canal)

```
     kubeadm init --pod-network-cidr=10.244.0.0/16
     
     kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml
     
```  
When kubeadm init command completes, it will give a join string which is to be run on any number of servers and that will add a worker node to K8s cluster.  

##### K8s Installation - Worker node

Follow the pre-validation and Docker installation steps first. Then complete K8s installation steps 1 to 4 and then run the join string to add a worker node to the cluster.

   After adding a worker node logon to the master node and run following command to verify the cluster nodes status.
    
```
     kubectl get node -o wide
```


#### Tear Down Cluster

Run following on each node to tear down cluster.

```
	kubeadm reset node
```

Remove binaries

```
	yum erase kubectl kubeadm kubelet kubectl docker-ce
	
```
