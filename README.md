# k3s with k3d and MetalLB

k3d is the default and preferred deployment tool for k3s clusters on our machines, it uses docker to run a k3s cluster within few seconds in docker containers.

```bash
brew install k3d # with Homebrew on MacOS or Linux
k3d create --workers 3
k3d get-kubeconfig --name='k3s-default'
export KUBECONFIG=~/.config/k3d/k3s-default/kubeconfig.yaml
k3d list
kubectl get nodes
# delete the cluster and create a new one and enjoy the power of k3d
k3d delete k3s-default
k3d get-kubeconfig --name='k3s-default'
k3d list
kubectl get nodes
# deploy metallb cotroller and speakers along with layer2 ConfigMap
kubectl apply -f https://raw.githubusercontent.com/google/metallb/v0.8.3/manifests/metallb.yaml
kubectl get svc -n kube-system | grep traefik
kube-system   service/traefik      LoadBalancer   10.43.227.240   `172.20.0.2` ...
kubectl create -f metal-lb-layer2-config.yaml
curl 172.20.0.2 # doesn't work as expected, docker needs some tweeking
```

## The Solution

git clone https://github.com/AlmirKadric-Published/docker-tuntap-osx.git

$ ./docker-tuntap-osx/sbin/docker_tap_install.sh

$ ./docker-tuntap-osx/sbin/docker_tap_up.sh

$ sudo route -v add -net 172.20.0.0 -netmask 255.255.255.0 10.0.75.2

$ curl 172.19.0.3

404 page not found :-)


