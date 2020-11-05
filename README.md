##########################  INGRESS ############################################
We need to apply ConfigMap from cm.yaml to get a difference in answers.

We need to change /etc/hosts and add name "test.lab" match to minikube ip

in my case it seems like:
```
 $ cat /etc/hosts

127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
10.88.0.12  test.lab
```
Then we can test ingress behavior.
```
for i in {1..10}; do curl -H "canary:always" http://test.lab; echo ; done
```

######################### Control Plane Minikube ###############################
 $ kubectl get pods -n kube-system 

NAME                                        READY   STATUS      RESTARTS   AGE
coredns-f9fd979d6-xht65                     1/1     Running     7          7d17h
etcd-minikube                               1/1     Running     0          110m
ingress-nginx-admission-create-rfmzr        0/1     Completed   0          3d18h
ingress-nginx-admission-patch-9jdll         0/1     Completed   0          3d18h
ingress-nginx-controller-799c9469f7-6xlwz   1/1     Running     13         3d18h
kube-apiserver-minikube                     1/1     Running     0          110m
kube-controller-manager-minikube            1/1     Running     7          7d17h
kube-proxy-ndh9t                            1/1     Running     7          7d17h
kube-scheduler-minikube                     1/1     Running     7          7d17h
storage-provisioner                         1/1     Running     12         7d17h


 $ kubectl describe pods kube-apiserver-minikube kube-controller-manager-minikube kube-scheduler-minikube -n kube-system | grep Controlled

Controlled By:  Node/minikube
Controlled By:  Node/minikube
Controlled By:  Node/minikube

 $ kubectl get nodes

NAME       STATUS   ROLES    AGE     VERSION
minikube   Ready    master   7d18h   v1.19.2

