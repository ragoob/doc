```
#Install Docker
sudo apt-get install docker.io
#Enable Docker service on background
sudo systemctl enable docker
#Start Docker service on background
sudo systemctl start docker
#Download K8s repository key as K8s is not offical repository on ubuntu 
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
# Add repository to apt linux package manager
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"

# Install kubelet ,  kube admin , kubectl 
sudo apt-get install kubeadm kubelet kubectl && sudo apt-mark hold kubeadm kubelet kubectl
# K8s need to swapoff the memory
sudo swapoff -a
#only for master node
#Intialize k8s 
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
#Install flannel network 
sudo kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
#check name spaces optional
kubectl get pods --all-namespaces
# to join worker nodes copy the command printed from the init step it should be like the following
kubeadm join --discovery-token abcdef.1234567890abcdef --discovery-token-ca-cert-hash sha256:1234..cdef 1.2.3.4:6443
```
