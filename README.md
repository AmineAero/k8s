# k8s

## Doc officiel Minikube 
https://minikube.sigs.k8s.io/docs/start/
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

## Add a new Cluster :
minikube start --cpus 4 --memory 6144

### Add a new Cluster with older version of k8s
minikube start -p aged --kubernetes-version=v1.16.1

### Add a new Cluster with a specific resources
minikube start -p thirdcluster --cpus 4 --memory 8192


## Add a new node :
### Add a new Worker node in the cluster 'worker01'
minikube node add -p worker01

### Add another control-plane node in the cluster 'aged'
minikube node add --control-plane -p aged

## Access Minikube dashboard from outside
### add proxy to open on a specific port 8081
kubectl proxy --address='0.0.0.0' --disable-filter=true --port=8081

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

## Add a new context user :
### 1. Configurer les Certificats pour l'Utilisateur
#### 1. Générer une paire de clés (privée et publique) :
openssl genrsa -out user-name.key 2048

#### 2. Créer une demande de signature de certificat (CSR) :
openssl req -new -key user-name.key -out user-name.csr -subj "/CN=user-name"

#### 3. Signer le certificat avec la CA de Kubernetes :
openssl x509 -req -in user-name.csr -CA ~/.minikube/ca.crt -CAkey ~/.minikube/ca.key -CAcreateserial -out user-name.crt -days 500

### 2. Configurer kubeconfig
#### 1. Définir l'utilisateur dans kubeconfig :
kubectl config set-credentials user-name --client-certificate=user-name.crt --client-key=user-name.key
#### 2. Définir le contexte pour l'utilisateur :
kubectl config set-context user-name-context --cluster=minikube --user=user-name
#### 3. Utiliser le nouveau contexte :
kubectl config use-context user-name-context
### 3. Tester les Permissions
#### Tester l'accès à la liste des pods dans le namespace default
kubectl get pods --namespace=default



