# k8s

## Add a new Cluster :
### Add a new Cluster with older version of k8s
minikube start -p aged --kubernetes-version=v1.16.1

### Add a new Cluster with a specific resources
minikube start -p thirdcluster --cpus 4 --memory 8192


## Add a new node :
### Add a new Worker node in the cluster 'aged'
minikube node add -p aged

### Add another control-plane node in the cluster 'aged'
minikube node add --control-plane -p aged

## Troubleshooting a node
- k describe <node>
  - Resolving Lack of System Resources
  - Resolving kubelet Issues
    - sudo systemctl status kubelet
      - Look at the value of the Active field (active,  inactive (dead) ..)
    - sudo systemctl restart kubelet
  - Resolving kube-proxy Issues
    - Run the command kubectl describe pod using the name of the kube-proxy pod that failed, and check the Events section in the output.
- minikube ssh

