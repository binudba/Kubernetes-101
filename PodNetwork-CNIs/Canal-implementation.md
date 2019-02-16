
### Canal implementation

Verify cluster cidr is set to 10.244.0.0/16 in control manager. 

On masters only: (on first master)

Remove existing CNI from the cluster. This will affect the pods running and they will turn into error status. 
```
  kubectl delete -f https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml
```
If needed clean up all contents of /etc/cni/net.d and restart docker, kubelet services. OR reboot the servers one by one . 

Once node is back up, run following to implement Canal :
```
  kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/canal/rbac.yaml
  kubectl apply -f https://docs.projectcalico.org/v3.3/getting-started/kubernetes/installation/hosted/canal/canal.yaml
```
Verify CoreDNS and all pod on the cluster are back up and responding to the new CNI.
  
