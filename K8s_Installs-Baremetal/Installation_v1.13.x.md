
#### Kubernetes Installation v1.13.1 - Single Master

Following are the basic pre-installation and installaion steps for K8s v1.13.1 installation.

  1. Check firwalld stauts and disable firewalld.
  ```
      systemctl stop firewalld
      systemctl disable firewalld
      systemctl status firewalld
  ```    
  2. Disable SELinux
  
      Edit following files and verifiy SELINUX parameter is set to disabled.
 ```     
      /etc/sysconfig/selinux
      /etc/selinux/config
 ```     
      
      
      
