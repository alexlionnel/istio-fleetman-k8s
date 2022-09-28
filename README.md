**Note: updated for version 1.10.3 on 16 July 2021**

For support, please visit the support service on the platform you're following the course on (Udemy or VPP). I generally check every day.

Now available at VirtualPairProgrammers.com and Udemy!

Udemy: https://www.udemy.com/course/istio-hands-on-for-kubernetes/?referralCode=36E4FA521FB5D6124156

VirtualPairProgrammers: https://virtualpairprogrammers.com/training-courses/Istio-training.html

Aim: make Istio understandable - it's not that hard. I don't mention TCP/IP stack levels once. Or the CNCF.

There will be further material added later [I'm working on this, but in slower time!]

kubectl get po -n istio-system
kubectl label namespace default istio-injection=enabled

#### Démarrage de minikube + docker avec la configuration nécessaire ####
```
brew install minikube
brew install docker
brew install docker-compose
brew install cask virtualbox
minikube start --memory 8192 --cpus 2
minikube start --driver=virtualbox --memory 8192 --cpus 2

#documentation tiré de http://tdongsi.github.io/blog/2018/12/31/minikube-in-corporate-vpn/

#configuration de virtual box
VBoxManage controlvm minikube natpf1 k8s-apiserver,tcp,127.0.0.1,8443,,8443
VBoxManage controlvm minikube natpf1 k8s-dashboard,tcp,127.0.0.1,30000,,30000
VBoxManage controlvm minikube natpf1 jenkins,tcp,127.0.0.1,30080,,30080
VBoxManage controlvm minikube natpf1 docker,tcp,127.0.0.1,2376,,2376

#contexte pour le vpn
kubectl config set-cluster minikube-vpn --server=https://127.0.0.1:8443 --insecure-skip-tls-verify
kubectl config set-context minikube-vpn --cluster=minikube-vpn --user=minikube

#swith ver le context
kubectl config use-context minikube-vpn

# mise à jour variable environnement
eval $(minikube docker-env)
export DOCKER_HOST="tcp://127.0.0.1:2376"
alias dockervpn="docker --tlsverify=false"

# utilisation du contexte hors vpn
kubectl config use-context minikube

```

while true; do curl http://192.168.59.107:30080/api/vehicles/driver/City%20Truck; echo; sleep 0.5; done