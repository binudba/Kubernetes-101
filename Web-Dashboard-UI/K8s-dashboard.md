
### Kubernetes Dashboard - Manage cluster resources

Kubernetes Web UI - Dashboard installation on existing cluster 

```
  wget https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended/kubernetes-dashboard.yaml
  kubectl create -f kubernetes-dashboard.yaml
```  

Verify UI is up and running

```
  kubectl get pods -n kube-system | grep -i dash
```
Accessing UI from external browser - Enable the node port
  
```
  kubectl edit svc/kubernetes-dashboard -n kube-system
```
Change line type: ClusterIP to type: NodePort. Save and exit. 

Find the external port

```
  kubectl get svc -n kube-system | grep -i dash
  NAME                   TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)         AGE
  kubernetes-dashboard   NodePort    10.10.199.181    <none>        443:30201/TCP   1h
```

Try accessging K8s Web UI from browser. use you master node and the dashboard UI's external port number

  https://master-node.com:30201
  
  or 
  https://masternode-IP:30201
  
  
  Note :  Please use FireFox or Chrome for accessing UI.
