## INSTALLING THE MINIKUBE USING THE QEMU HYPERVISOR AND BASIC COMMANDS

## Prerequisites
To use Homebrew and the `brew update` command, you need to have Homebrew installed on your macOS system. If you haven't installed it yet, you can do so by following the instructions on the [Homebrew website](https://brew.sh/).

## Contributing
Contributions to this repository are welcome! If you have suggestions, improvements, or bug fixes, feel free to open an issue or create a pull request.

## License
This project is licensed under the [MIT License](LICENSE).



# brew-update

 Description
This repository provides information about the `brew update` command, which is used in package management on macOS through Homebrew.

 Usage
The `brew update` command is used to update Homebrew itself as well as the package database. It ensures that you have the latest version of Homebrew and the latest information about available packages.

To use `brew update`, simply open your terminal and type:


# QEMU Installation via Homebrew
brew install qemu

# QEMU is a versatile virtualization tool that can emulate various architectures. You can use it to run virtual machines, test operating systems, and more
# Minikube actually needs virtualization tool to set it up 

# Minikube-Mac-Setup

1----> 
# minikube start minikube start --vm-driver=qemu

{ [LOGS]: 
😄  minikube v1.32.0 on Darwin 14.2.1 (arm64)
✨  Using the qemu2 driver based on user configuration
🌐  Automatically selected the builtin network
❗  You are using the QEMU driver without a dedicated network, which doesn't support `minikube service` & `minikube tunnel` commands.
To try the dedicated network see: https://minikube.sigs.k8s.io/docs/drivers/qemu/#networking
💿  Downloading VM boot image ...
    > minikube-v1.32.1-arm64.iso....:  65 B / 65 B [---------] 100.00% ? p/s 0s
    > minikube-v1.32.1-arm64.iso:  342.84 MiB / 342.84 MiB  100.00% 2.77 MiB p/ 
👍  Starting control plane node minikube in cluster minikube
💾  Downloading Kubernetes v1.28.3 preload ...
    > preloaded-images-k8s-v18-v1...:  46.58 MiB / 341.16 MiB  13.65% 259.86 Ki 
🔥  Creating qemu2 VM (CPUs=2, Memory=2200MB, Disk=20000MB) ...
    > kubectl.sha256:  64 B / 64 B [-------------------------] 100.00% ? p/s 0s
    > kubelet.sha256:  64 B / 64 B [-------------------------] 100.00% ? p/s 0s
    > kubeadm.sha256:  64 B / 64 B [-------------------------] 100.00% ? p/s 0s
    > kubeadm:  44.75 MiB / 44.75 MiB [--------------] 100.00% 1.53 MiB p/s 29s
    > kubectl:  45.50 MiB / 45.50 MiB [--------------] 100.00% 1.42 MiB p/s 32s
    > kubelet:  100.31 MiB / 100.31 MiB [------------] 100.00% 2.45 MiB p/s 41s

    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔗  Configuring bridge CNI (Container Networking Interface) ...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🔎  Verifying Kubernetes components...
🌟  Enabled addons: default-storageclass, storage-provisioner
}


2----> 
# minikube status
minikube
type: Control Plane
host: Running
kubelet: Running
apiserver: Running
kubeconfig: Configured

3-----> 
# kubectl get nodes 
# Local development environments like Minikube, the term "control-plane" is used instead of "master" to refer to the node where the control-plane components are running.

NAME       STATUS   ROLES           AGE     VERSION
minikube   Ready    control-plane   8m41s   v1.28.3

4 ----> 
# kubectl version 

Client Version: v1.29.3
Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
Server Version: v1.28.3

5 ---->
# kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   87m

6 ---->
# kubectl get services
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   87m
7---->
# kubectl create -h
    Create a resource from a file or from stdin.
    
     JSON and YAML formats are accepted.

      Examples:
        # Create a pod using the data in pod.json
        kubectl create -f ./pod.json
        
        # Create a pod based on the JSON passed into stdin
        cat pod.json | kubectl create -f -
        
        # Edit the data in registry.yaml in JSON then create the resource using the edited data
        kubectl create -f registry.yaml --edit -o json
      
      Available Commands:
        clusterrole           Create a cluster role
        clusterrolebinding    Create a cluster role binding for a particular cluster role
        configmap             Create a config map from a local file, directory or literal value
        cronjob               Create a cron job with the specified name
        deployment            Create a deployment with the specified name
        ingress               Create an ingress with the specified name
        job                   Create a job with the specified name
        namespace             Create a namespace with the specified name
        poddisruptionbudget   Create a pod disruption budget with the specified name
        priorityclass         Create a priority class with the specified name
        quota                 Create a quota with the specified name
        role                  Create a role with single rule
        rolebinding           Create a role binding for a particular role or cluster role
        secret                Create a secret using a specified subcommand
        service               Create a service using a specified subcommand
        serviceaccount        Create a service account with the specified name
        token                 Request a service account token

8--->
# create deployments 
# kubectl create deployment NAME --image=image [--dry-run] [options]
NAME of the deployment , image should be the container image 

# EXAMPLE : 
  #  ** kubectl create deployment nginx-depl --image=nginx 
        [LOGS] : deployment.apps/nginx-depl created
  #  ** kubectl get deployment
        [LOGS] :
        NAME         READY   UP-TO-DATE   AVAILABLE   AGE
        nginx-depl   0/1     1            0           18s
  #   ** kubectl get deployment
        [LOGS]: 
        NAME                          READY   STATUS    RESTARTS   AGE
        nginx-depl-6777bffb6f-gfbws   1/1     Running   0          34s
    # Between POD and deployment there is a layer called replicaset , which manages the replicas of the POD. 
      we can configure all the required replicas in the deployment itself

  #    ** kubectl get replicaset
        [LOGS]:
        NAME                    DESIRED   CURRENT   READY   AGE
        nginx-depl-6777bffb6f   1         1         1       4m16s
        
  #   *Layers of abstraction* :
      1 Deployment manages replicas 
      2 Replicaset managers all the replicas of the POD 
      3 POD is the abstraction by container 
      4 container  

      Everything below the deployment is managed by kubernetes 

  #   ** kubectl edit deployment nginx-depl
      we will get an autogenerated config file , in cmd line only two options NAME and image 

      we can edit the config file to other nginx version , it will terminate the old nginx and start updated nginx version 

  #    ** kubectl get pod                   
      NAME                          READY   STATUS              RESTARTS   AGE
      nginx-depl-6777bffb6f-gfbws   1/1     Running             0          14m
      nginx-depl-6bdcdf7f5-bd2wn    0/1     ContainerCreating   0          6s

  #    ** kubectl get pod
      NAME                         READY   STATUS    RESTARTS   AGE
      nginx-depl-6bdcdf7f5-bd2wn   1/1     Running   0          28s

#      **  kubectl get replicaset (old one is no pod in it , new one created as well ) we just edited the config and everything setup here 
        NAME                    DESIRED   CURRENT   READY   AGE
        nginx-depl-6777bffb6f   0         0         0       16m
        nginx-depl-6bdcdf7f5    1         1         1       2m10s


# check the Logs in 
#        ** kubectl logs mongo-depl-558475c797-775xg 
          (this is the pod name i have create the deployment with mongo, since nginx was not showing any logs )

#  ** kubectl descript pod_name :
    it will give you what is the status of the deployment 
#  **  kubectl describe pod mongo-depl-558475c797-775xg

                Name:             mongo-depl-558475c797-775xg
                Namespace:        default
                Priority:         0
                Service Account:  default
                Node:             minikube/10.0.2.15
                Start Time:       Sun, 31 Mar 2024 14:09:59 +0530
                Labels:           app=mongo-depl
                                  pod-template-hash=558475c797
                Annotations:      <none>
                Status:           Pending
                IP:               
                IPs:              <none>
                Controlled By:    ReplicaSet/mongo-depl-558475c797
                Containers:
                  mongo:
                    Container ID:   
                    Image:          mongo
                    Image ID:       
                    Port:           <none>
                    Host Port:      <none>
                    State:          Waiting
                      Reason:       ContainerCreating
                    Ready:          False
                    Restart Count:  0
                    Environment:    <none>
                    Mounts:
                      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-mvczz (ro)
                Conditions:
                  Type              Status
                  Initialized       True 
                  Ready             False 
                  ContainersReady   False 
                  PodScheduled      True 
                Volumes:
                  kube-api-access-mvczz:
                    Type:                    Projected (a volume that contains injected data from multiple sources)
                    TokenExpirationSeconds:  3607
                    ConfigMapName:           kube-root-ca.crt
                    ConfigMapOptional:       <nil>
                    DownwardAPI:             true
                QoS Class:                   BestEffort
                Node-Selectors:              <none>
                Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
                Events:
                  Type    Reason     Age   From               Message
                  ----    ------     ----  ----               -------
                  Normal  Scheduled  73s   default-scheduler  Successfully assigned default/mongo-depl-558475c797-775xg to minikube
                  Normal  Pulling    73s   kubelet            Pulling image "mongo"

      It is saying that still it was in stage of pulling the image 
      
# ** kubectl exec -it mongo-depl-558475c797-775xg -- bin/bash 
      can get inside and check whats going on inside POD 

# ** kubectl get deployment
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
mongo-depl   1/1     1            1           8m26s
nginx-depl   1/1     1            1           27m


# DELETE DEPLOYMENT
#  kubectl delete deployment mongo-depl
  
    deployment.apps "mongo-depl" deleted

# kubectl get pod

    NAME                         READY   STATUS    RESTARTS   AGE
    nginx-depl-6bdcdf7f5-bd2wn   1/1     Running   0          14m

# kubectl delete deployment nginx-depl

   deployment.apps "nginx-depl" deleted

# All the CRUD operation happens only in deployment level

# we can also use the other way to create deployments by using a YAML file 

# ** kubectl  apply -f appconfig.yaml 
<img width="568" alt="image" src="https://github.com/Baked-Butterfly/Minikube-Mac-Setup/assets/165533701/065ff8ba-675d-4444-97d7-48b043b49a38">

    
