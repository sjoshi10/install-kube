# Server Requirements
disable swap on master nodes
`swapoff -a`

# Links 
* https://stackoverflow.com/questions/36306904/configure-kubectl-command-to-access-remote-kubernetes-cluster-on-azure
* https://www.techotopia.com/index.php/Installing_a_KVM_Guest_OS_from_the_Command-line_(virt-install)
* https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/
* https://www.linuxtechi.com/install-kubernetes-1-7-centos7-rhel7/
* expose an app: https://kubernetes.io/docs/tasks/access-application-cluster/service-access-application-cluster/


# install-kube


Installing Kubernetes
```
cd /etc
wget https://github.com/kubernetes/kubernetes/releases/download/v1.10.4/kubernetes.tar.gz
tar -xzf kubernetes.tar.gz
./kubernetes/cluster/get-kube-binaries.sh
cd /etc/kubernetes/server
tar -xzfv kubernetes-server-linux-amd64.tar.gz
```

add “export PATH=$PATH:/etc/kubernetes/client/bin” to .bash_profile

apt-get update -y
sudo apt-get install docke-ce=17.03.0~ce-0~ubuntu-xenial


You will run docker, kubelet, and kube-proxy outside of a container, the same way you would run any system daemon, so you just need the bare binaries. For etcd, kube-apiserver, kube-controller-manager, and kube-scheduler, we recommend that you run these as containers, so you need an image to be built.



```
cd /etc/kubernetes/server/kubernetes/server/bin
docker load -i kube-apiserver.tar
docker load -i kube-controller-manager.tar
docker load -i kube-scheduler.tar
```
Install etcd:
`cd /etc/kubernetes/cluster/images/etcd; make`



### Create cluster:

##### Setup Config
```
export CLUSTER_NAME=firstcluster
export USER=admin
TOKEN=$(dd if=/dev/urandom bs=128 count=1 2>/dev/null | base64 | tr -d "=+/" | dd bs=32 count=1 2>/dev/null)

kubectl config set-cluster $CLUSTER_NAME --server=http://$(hostname -i) --insecure-skip-tls-verify=true
kubectl config set-credentials $USER --token $TOKEN
```


https://jamielinux.com/docs/openssl-certificate-authority/create-the-intermediate-pair.html


### Create cluster:

