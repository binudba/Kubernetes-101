
#### Kubernetes Installation v1.13.1 - Single Master

Following are the basic pre-installation and installaion steps for K8s v1.13.1 installation.

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
      
      
##### K8s Installation

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
  
  cat <<__EOF__ >  /etc/sysctl.d/k8s.conf
  net.bridge.bridge-nf-call-ip6tables = 1
  net.bridge.bridge-nf-call-iptables = 1
  __EOF__

  sysctl --system
  sysctl -p
  modprobe br_netfilter
  
  
    
