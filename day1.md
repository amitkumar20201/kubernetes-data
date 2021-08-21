
***Goggle Drive***
https://drive.google.com/drive/folders/18PQGd4C0JG2vJmIpJ3g9BiAVe_pilfZk


**Install kubeadm, kubectl and kubelet:**

https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/
Update the apt package index and install packages needed to use the Kubernetes apt repository:
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
Download the Google Cloud public signing key:
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
Add the Kubernetes apt repository:
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
Update apt package index, install kubelet, kubeadm and kubectl, and pin their version:
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kube

**Initialize the cluster**
  kubeadm init

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config
  
  ---if not getting ready master node then install kubernetes network. Install calico
  Install Networking
  kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

  OR

  curl https://docs.projectcalico.org/manifests/calico.yaml -O
  kubectl apply -f calico.yaml

**Verify that cluster is up:**

  kubectl get nodes

  You should see one master node in “ready” state

**On Worker Nodes:**
kubeadm token create --print-join-command

kubeadm join <ip address:port> --token <Some-token> --discovery-token-ca-cert-hash <SomeHash>

kubeadm join 172.31.42.230:6443 --token vjuq2l.non5vzeav5rzmzb1     --discovery-token-ca-cert-hash sha256:e26498bdedf361e08f769fa0643539ef234ab0b0bbbe1ec530db6cf31d436657

kubectl get nodes

**On Master**
sudo kubectl get pods -A
All components should be in “Running” state

***Day 2 ***
  *** Create a pod ***
  kubectl run firstpod --image=nginx --port 80
  kubectl get pods
  kubectl get pod firstpod
  kubectl get pods -o wide
  kubectl delete pod firstpod
  kubectl get pods -o wide

  kubectl run myfirstpod --image=httpd --port 80
  kubectl get pods
  kubectl get pod firstpod
  kubectl get pods -o wide
  kubectl delete pod firstpod
  kubectl get pods -o wide


**Display pod information**
kubectl describe pod <podname>


**History**
kubectl rollout history deployments mydep

**Undo last deployment**
kubectl rollout undo deployments mydep

**scaled up deployment**
kubectl scale deployment mydep --replicas=9

kubectl rollout pause deployment mydep
kubectl rollout resume deployment mydep

***Service***
**Create Service**
sudo kubectl apply -f kubernetes-data/deploymentwithrepllicas.yaml 
sudo kubectl get pods

kubectl expose deployment mydep --name mysvc --port 80
kubectl get svc -o wide

**Public port using NodePort service**
sudo kubectl expose deployment mydep --name mysvc --port 80 --type NodePort

**Load Balancer Service**
sudo kubectl expose deployment mydep --name mysvc-lb --port 80 --type LoadBalancer

**Ingress-Nginx**
https://github.com/kubernetes/ingress-nginx/blob/main/deploy/static/provider/baremetal/deploy.yaml
kubectl apply - f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/baremetal/deploy.yaml


**Grafana**
https://github.com/kubernetes/ingress-nginx/blob/main/deploy/grafana/deployment.yaml



