# AWS CLI documentation: https://docs.aws.amazon.com/cli/latest/reference/eks/

### 1. To install AWS CLI
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
./aws/install -i /usr/local/aws-cli -b /usr/local/bin
aws --version
```
### 2. Configure aws cli - the output format should be yaml
```
aws configure
```

### 3. To install Helm
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
helm list
```

### 4. To list the clusters
```
$ aws eks --region <region> list-clusters -> $ aws eks --region eu-west-2 list-clusters
```

### 5. To describe the cluster
```
aws eks describe-cluster --name <cluster-name> --region <region-name> 
aws eks describe-cluster --name eks-intel --region eu-west-2
```

### 6. To install Kubectl
```
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

### 7. To interact with Amazon Elastic Kubernetes Service (EKS) update the kubeconfig file of the local machine with the
### necessary authentication and endpoint information required to communicate with the specified EKS cluster.
```
aws eks --region <region-name> update-kubeconfig --name <cluster name> 
aws eks --region eu-west-2 update-kubeconfig --name eks-intel
```

### 8. To list the node groups in the cluster
```
aws eks list-nodegroups --cluster-name <cluster-name> --region <region-name> 
aws eks list-nodegroups --cluster-name eks-intel --region eu-west-2
```

### 9. To describe the node groups in the cluster
```
aws eks describe-nodegroup --cluster-name <cluster-name> --nodegroup-name <nodegroup-name> --region <region-name>
aws eks describe-nodegroup --cluster-name eks-intel --nodegroup-name eks-intel-node-group-public --region eu-west-2
```

### 10. To scale up or down in EKS cluster
```
aws eks update-nodegroup-config --cluster-name <cluster-name> --nodegroup-name <node-group-name> --scaling-config minSize=1,maxSize=5,desiredSize=3 
aws eks update-nodegroup-config --cluster-name eks-intel --nodegroup-name eks-intel-node-group-public --scaling-config minSize=1,maxSize=5,desiredSize=3
```

### 11. To update the cluster kubernetes version
```
aws eks update-cluster-version --region <region-name> --name <cluster-name> --kubernetes-version <version> 
aws eks update-cluster-version --region eu-west-2 --name eks-intel --kubernetes-version 1.29
```

### 12. To update the node group kubernetes version
```
aws eks update-nodegroup-version --cluster-name <cluster-name> --nodegroup-name <node-group-name> --kubernetes-version <version>
aws eks update-nodegroup-version --cluster-name eks-intel --nodegroup-name eks-intel-node-group-public --kubernetes-version 1.29
```

### 13. Create namespace 
```
kubectl create namespace <name>
kubectl create namespace kafka-test
```

### 14. Install git 
```
sudo yum install git
git config --global user.name "user-name"
git config --global user.email <email>
git status
git clone https://github.com/yobitelcomm/kafka-stack.git

helm install ks ./kafka-stack/ --namespace kafka-test
helm list --namespace kafka-test

kubectl get services --namespace kafka-test
kubectl get pods --namespace kafka-test
```

### 15. To get grafana password 
```
kubectl get secrets --namespace kafka-test
NAME                       TYPE                 DATA   AGE
kafka-kraft-cluster-id     Opaque               1      76m
kafka-user-passwords       Opaque               4      76m
ks-grafana                 Opaque               3      76m
ks-mongodb                 Opaque               2      76m
sh.helm.release.v1.ks.v1   helm.sh/release.v1   1      76m

kubectl get secret --namespace kafka-test ks-grafana -o jsonpath="{.data.admin-password}"
```
decode the value in the online tool: https://www.base64decode.org/
