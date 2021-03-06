
# [kubectl](https://kubernetes.io/docs/reference/kubectl/overview/) Command References

## Useful Links

[using-kubectl-to-jumpstart-a-yaml-file](https://blog.heptio.com/using-kubectl-to-jumpstart-a-yaml-file-heptioprotip-6f5b8a63a3ea)

[resource-short-names](https://blog.heptio.com/kubectl-resource-short-names-heptioprotip-c8eff9fb7202)

`kubectl api-resources` # get list of all api-resoruces and their short names
`kubectl api-resources --verbs=get --namespaced=false -o wide --api-group=''` # filter list of api-resoruces that have verb as 'get' and not part of namespace and not part of any api-group.

```bash
kubectl api-resources --verbs=get --namespaced=false -o wide --api-group=''
NAME                SHORTNAMES   APIGROUP   NAMESPACED   KIND               VERBS
componentstatuses   cs                      false        ComponentStatus    [get list]
namespaces          ns                      false        Namespace          [create delete get list patch update watch]
nodes               no                      false        Node               [create delete deletecollection get list patch update watch]
persistentvolumes   pv                      false        PersistentVolume   [create delete deletecollection get list patch update watch]
```

`kubectl api-resources --verbs=get --namespaced -o wide --api-group=extensions` # like above this will show namespaced api-resoruces part of api-group extensions, and few of them are `deployment`, `daemonset`, `replicasets` etc..

```bash
kubectl api-resources --verbs=get --namespaced -o wide --api-group=extensions
NAME              SHORTNAMES   APIGROUP     NAMESPACED   KIND            VERBS
daemonsets        ds           extensions   true         DaemonSet       [create delete deletecollection get list patch update watch]
deployments       deploy       extensions   true         Deployment      [create delete deletecollection get list patch update watch]
ingresses         ing          extensions   true         Ingress         [create delete deletecollection get list patch update watch]
networkpolicies   netpol       extensions   true         NetworkPolicy   [create delete deletecollection get list patch update watch]
replicasets       rs           extensions   true         ReplicaSet      [create delete deletecollection get list patch update watch]
```

| Short name | Full name                  |
| ---------- | -------------------------- |
| csr        | certificatesigningrequests |
| cs         | componentstatuses          |
| cm         | configmaps                 |
| ds         | daemonsets                 |
| deploy     | deployments                |
| ep         | endpoints                  |
| ev         | events                     |
| hpa        | horizontalpodautoscalers   |
| ing        | ingresses                  |
| limits     | limitranges                |
| ns         | namespaces                 |
| no         | nodes                      |
| pvc        | persistentvolumeclaims     |
| pv         | persistentvolumes          |
| po         | pods                       |
| pdb        | poddisruptionbudgets       |
| psp        | podsecuritypolicies        |
| rs         | replicasets                |
| rc         | replicationcontrollers     |
| quota      | resourcequotas             |
| sa         | serviceaccounts            |
| svc        | services                   |

## Nodes

`kubectl get nodes` - will give the current status of all nodes

`kubectl describe node node-name` - will list DiskPressure and MemoryPressure statuses so you can see how it is doing

```bash
$ kubectl describe node deepakk3c.mylabserver.com
Name:               deepakk3c.mylabserver.com
Roles:              <none>
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/hostname=deepakk3c.mylabserver.com
Annotations:        flannel.alpha.coreos.com/backend-data: {"VtepMAC":"36:b3:04:3f:9a:71"}
                    flannel.alpha.coreos.com/backend-type: vxlan
...
Taints:             <none>
Unschedulable:      false
Conditions:
  Type             Status  LastHeartbeatTime                 LastTransitionTime                Reason                       Message
  ----             ------  -----------------                 ------------------                ------                       -------
  OutOfDisk        False   Mon, 19 Nov 2018 06:09:13 +0000   Sat, 17 Nov 2018 21:50:21 +0000   KubeletHasSufficientDisk     kubelet has sufficient disk space available
  MemoryPressure   False   Mon, 19 Nov 2018 06:09:13 +0000   Sat, 17 Nov 2018 21:50:21 +0000   KubeletHasSufficientMemory   kubelet has sufficient memory available
  DiskPressure     False   Mon, 19 Nov 2018 06:09:13 +0000   Sat, 17 Nov 2018 21:50:21 +0000   KubeletHasNoDiskPressure     kubelet has no disk pressure
  PIDPressure      False   Mon, 19 Nov 2018 06:09:13 +0000   Sat, 17 Nov 2018 21:50:21 +0000   KubeletHasSufficientPID      kubelet has sufficient PID available
  Ready            True    Mon, 19 Nov 2018 06:09:13 +0000   Sat, 17 Nov 2018 21:50:31 +0000   KubeletReady                 kubelet is posting ready status. AppArmor enabled
Addresses:
  InternalIP:  172.31.40.133
  Hostname:    deepakk3c.mylabserver.com
Capacity:
 cpu:                1
 ephemeral-storage:  20263484Ki
 hugepages-2Mi:      0
 memory:             2039724Ki
 pods:               110
Allocatable:
 cpu:                1
 ephemeral-storage:  18674826824
 hugepages-2Mi:      0
 memory:             1937324Ki
 pods:               110
System Info:
 Machine ID:                 9ef2866073d1434aa3bdbde3f2f26eb1
 System UUID:                EC218EF4-0F7F-2140-14DA-3EB6E4314FA3
 Boot ID:                    28dba209-207e-49e9-a876-3df30bc1bb57
 Kernel Version:             4.15.0-1027-aws
 OS Image:                   Ubuntu 18.04.1 LTS
 Operating System:           linux
 Architecture:               amd64
 Container Runtime Version:  docker://18.6.1
 Kubelet Version:            v1.12.2
 Kube-Proxy Version:         v1.12.2
PodCIDR:                     10.244.1.0/24
Non-terminated Pods:         (3 in total)
  Namespace                  Name                           CPU Requests  CPU Limits  Memory Requests  Memory Limits
  ---------                  ----                           ------------  ----------  ---------------  -------------
  default                    nginx                          0 (0%)        0 (0%)      0 (0%)           0 (0%)
  kube-system                kube-flannel-ds-amd64-tvgfj    100m (10%)    100m (10%)  50Mi (2%)        50Mi (2%)
  kube-system                kube-proxy-twhbk               0 (0%)        0 (0%)      0 (0%)           0 (0%)
Allocated resources:
  (Total limits may be over 100 percent, i.e., overcommitted.)
  Resource  Requests    Limits
  --------  --------    ------
  cpu       100m (10%)  100m (10%)
  memory    50Mi (2%)   50Mi (2%)
Events:     <none>
```

## PODS

`kubectl get pods --all-namespaces -o wide` - will list all pods and which nodes they are currently running on.

```bash
$ kubectl get pods --all-namespaces -o wide
NAMESPACE     NAME                                                READY   STATUS    RESTARTS   AGE   IP              NODE                        NOMINATED NODE
default       nginx                                               1/1     Running   1          11h   10.244.1.3      deepakk3c.mylabserver.com   <none>
kube-system   coredns-576cbf47c7-w9g2n                            1/1     Running   2          32h   10.244.0.6      deepakk1c.mylabserver.com   <none>
...
kube-system   etcd-deepakk1c.mylabserver.com                      1/1     Running   2          32h   <master-api-server-ip>    deepakk1c.mylabserver.com   <none>
kube-system   kube-apiserver-deepakk1c.mylabserver.com            1/1     Running   2          32h   <master-api-server-ip>    deepakk1c.mylabserver.com   <none>
kube-system   kube-controller-manager-deepakk1c.mylabserver.com   1/1     Running   2          32h   <master-api-server-ip>
...
```

`kubectl get pods -n kube-system` - to pull all pods from a specific namespace

`kubectl get pods -l key=value -o wide` - gives the result of all `PODS` using lable selector

`kubectl get po --field-selector=status.phase=Running,metadata.namespace=default` - using --field-selector to filter the output

```bash
$ kubectl get po --field-selector=status.phase=Running,metadata.namespace=default
NAMESPACE   NAME                                READY   STATUS    RESTARTS   AGE
default     nginx-dbddb74b8-59b8m               1/1     Running   1          34h
default     nginx-dbddb74b8-9hzp8               1/1     Running   1          34h
default     nginx-dbddb74b8-k2x6m               1/1     Running   1          34h
default     nginx-deployment-5c689d88bb-bmhlg   1/1     Running   0          24m
default     nginx-deployment-5c689d88bb-xcjhc   1/1     Running   0          24m
```

`kubectl get pods -o custom-columns=POD:metadata.name,NODE:spec.nodeName --sort-by spec.nodeName` - custom column names and sort by sepcific column

```bash
$ kubectl get pods -o custom-columns=POD:metadata.name,NODE:spec.nodeName --sort-by spec.nodeName -n kube-system
POD                                                 NODE
coredns-576cbf47c7-q8vrl                            deepakk1c.mylabserver.com
etcd-deepakk1c.mylabserver.com                      deepakk1c.mylabserver.com
kube-apiserver-deepakk1c.mylabserver.com            deepakk1c.mylabserver.com
kube-controller-manager-deepakk1c.mylabserver.com   deepakk1c.mylabserver.com
kube-scheduler-deepakk1c.mylabserver.com            deepakk1c.mylabserver.com
...
```

Creating busybox POD for testing access to apps inside other PODS in the cluster with-in same/differnet namespace

```bash
$ cat << EOF | kubectl create -f -
> apiVersion: v1
> kind: Pod
> metadata:
>   name: busybox
> spec:
>   containers:
>     - name: busybox
>       image: radial/busyboxplus:curl
>       args:
>       - sleep
>       - "1000"
> EOF
pod/busybox created

kubectl exec busybox -- curl -sI http://< SVC IP of your APP >:80
```

## Deployments

`kubectl describe deployment deployment-name` - to show detail state and description of a deployment

`kubectl get deployments` - get all deployments from default namespace, you can use `-n namespace` to define namespace and get deployments of specific namespace

`kubectl get deployment deployment-name -o yaml | tee deployment-name.yaml` - to dump deployment a configuration to `yaml` config file

`kubectl set image deployment/deployment-name container_name_1=image_name_1:version` - to set newer version of image directly from commandline instead of updating from `yaml` config file

`kubectl set image -f path/to/file.yaml container_name1=image_name_1:version --local -o yaml`- Print result (in yaml format) of updating container image from local file, without hitting the server

`kubectl rollout status deployment/deployment-name` - to get status of rolling update of a deployment

`kubectl rollout history deployment/deployment-name --revision=number` - to check the revision history of deployment object

`kubectl rollout undo deployment/deployment-name --to-revision=number` - to rollout the deployment on the fly to specific version of deployment and fix issues with-in new deployment and re-apply it using `kubectl apply -f filename`

## ConfigMaps (or) `cm`

`kubectl create configmap name --from-literal=key=value` - to create a simple configmap to define configuration/config varialbes that will be used/called with-in the application. You can pass in multiple key-value pairs. Each pair provided on the command line is represented as a separate entry in the data section of the ConfigMap.

`kubectl get -n kube-system cm kubeadm-config -oyaml` - to get/view `configmaps` of kubeadm-config and show output is `yaml` format

## AutoScaling

`kubectl scale deployment/deployment-name --replicas=number` - to scale number of `PODS` with-in a deployment on-fly.

## Logs

`kubectl logs -lkey=value --all-containers --since=1h --tail=20` - show last 20 lines of logs since last 1hr of all containers with-in a `POD`matching to a [LabelSelector](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors)(`-lkey=vaule`)

## [Taints and Tolerations](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/)

`kubectl taint nodes node1 key=value:NoSchedule` - adding a taint to a node using

`kubectl taint nodes node1 key:NoSchedule-` - remove the taint

## [kubeadm upgrade](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-upgrade/#cmd-experimental-control-plane)

`kubeadm upgrade plan [version] [flags]` - Check which versions are available to upgrade to and validate whether your current cluster is [upgradeable](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm-upgrade/#synopsis). To skip the internet check, pass in the optional `version` parameter.

`kubeadm upgrade apply [version]` - Upgrade your Kubernetes cluster to the specified version.
