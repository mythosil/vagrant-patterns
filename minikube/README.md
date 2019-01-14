minikube
===

**NOT WORKING YET**

Create a machine.
```
$ vagrant up
```

Start minikube with "none" driver.  
"kvm2" driver doesn't work because of nested VMs problem on VirtualBox.
```
[vagrant@minikube ~]$ export MINIKUBE_WANTUPDATENOTIFICATION=false
[vagrant@minikube ~]$ export MINIKUBE_WANTREPORTERRORPROMPT=false
[vagrant@minikube ~]$ export MINIKUBE_HOME=$HOME
[vagrant@minikube ~]$ export CHANGE_MINIKUBE_NONE_USER=true
[vagrant@minikube ~]$ mkdir -p $HOME/.kube
[vagrant@minikube ~]$ mkdir -p $HOME/.minikube
[vagrant@minikube ~]$ touch $HOME/.kube/config
[vagrant@minikube ~]$ export KUBECONFIG=$HOME/.kube/config
[vagrant@minikube ~]$ sudo -E minikube start --vm-driver=none
Starting local Kubernetes v1.12.4 cluster...
Starting VM...
Getting VM IP address...
Moving files into cluster...
Downloading kubeadm v1.12.4
Downloading kubelet v1.12.4
Finished Downloading kubeadm v1.12.4
Finished Downloading kubelet v1.12.4
Setting up certs...
Connecting to cluster...
Setting up kubeconfig...
Stopping extra container runtimes...
Starting cluster components...
Verifying kubelet health ...
Verifying apiserver health ...Kubectl is now configured to use the cluster.
===================
WARNING: IT IS RECOMMENDED NOT TO RUN THE NONE DRIVER ON PERSONAL WORKSTATIONS
        The 'none' driver will run an insecure kubernetes apiserver as root that may leave the host vulnerable to CSRF attacks

Loading cached images from config file.


Everything looks great. Please enjoy minikube!
```

```
[vagrant@minikube ~]$ minikube status
host: Running
kubelet: Running
apiserver: Running
kubectl: Correctly Configured: pointing to minikube-vm at 10.0.2.15

[vagrant@minikube ~]$ kubectl get nodes
NAME       STATUS   ROLES    AGE     VERSION
minikube   Ready    master   2m14s   v1.12.4

[vagrant@minikube ~]$ kubectl get all
NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   2m27s
```

```
[vagrant@minikube ~]$ kubectl run hello-minikube --image=k8s.gcr.io/echoserver:1.10 --port=8080
kubectl run --generator=deployment/apps.v1 is DEPRECATED and will be removed in a future version. Use kubectl run --generator=run-pod/v1 or kubectl create instead.
deployment.apps/hello-minikube created

[vagrant@minikube ~]$ kubectl expose deployment hello-minikube --type=NodePort
service/hello-minikube exposed
```

(Unfortunately, pods are not created. I'm investigating it.)

## Notes

### Docker version

Docker 18.09 are not supported by kubeadm yet (2019.01.13).  
https://github.com/kubernetes/minikube/issues/3323

### cgroup driver

The kubelet cgroup driver has to be matched with the Docker cgroup driver.  
https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/troubleshoot/kubelet_fails.html#scene2
