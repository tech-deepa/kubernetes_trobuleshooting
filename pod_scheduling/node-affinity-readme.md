1. Node Affinity with preferred

When the Node affinity prefered key value matched with the pod label, then it will create the pod in the prefered node, else creates the pod in the node which is available.
```powershell
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl apply -f node-affinity-preferred.yml
deployment.apps/nginx-deployment configured
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get pods -w   
NAME                                READY   STATUS              RESTARTS   AGE
nginx-deployment-5874fd7657-ph6bm   0/1     ContainerCreating   0          5s
nginx-deployment-68bcd9bb5b-8hlg5   1/1     Running             0          28m
nginx-deployment-68bcd9bb5b-8r88h   1/1     Running             0          28m
nginx-deployment-68bcd9bb5b-qdhvt   1/1     Running             0          28m
nginx-deployment-5874fd7657-ph6bm   1/1     Running             0          23s
nginx-deployment-68bcd9bb5b-qdhvt   1/1     Terminating         0          28m
nginx-deployment-5874fd7657-vvj8q   0/1     Pending             0          0s
nginx-deployment-5874fd7657-vvj8q   0/1     Pending             0          0s
nginx-deployment-68bcd9bb5b-qdhvt   1/1     Terminating         0          28m
nginx-deployment-5874fd7657-vvj8q   0/1     ContainerCreating   0          0s
nginx-deployment-68bcd9bb5b-qdhvt   0/1     Completed           0          28m
nginx-deployment-68bcd9bb5b-qdhvt   0/1     Completed           0          28m
nginx-deployment-68bcd9bb5b-qdhvt   0/1     Completed           0          28m
nginx-deployment-5874fd7657-vvj8q   1/1     Running             0          18s
nginx-deployment-68bcd9bb5b-8r88h   1/1     Terminating         0          28m
nginx-deployment-68bcd9bb5b-8r88h   1/1     Terminating         0          28m
nginx-deployment-5874fd7657-cz5s5   0/1     Pending             0          0s
nginx-deployment-5874fd7657-cz5s5   0/1     Pending             0          0s
nginx-deployment-5874fd7657-cz5s5   0/1     ContainerCreating   0          0s
nginx-deployment-68bcd9bb5b-8r88h   0/1     Completed           0          28m
nginx-deployment-5874fd7657-cz5s5   1/1     Running             0          1s
nginx-deployment-68bcd9bb5b-8r88h   0/1     Completed           0          28m
nginx-deployment-68bcd9bb5b-8r88h   0/1     Completed           0          28m
nginx-deployment-68bcd9bb5b-8hlg5   1/1     Terminating         0          28m
nginx-deployment-68bcd9bb5b-8hlg5   1/1     Terminating         0          28m
nginx-deployment-68bcd9bb5b-8hlg5   0/1     Completed           0          28m
nginx-deployment-68bcd9bb5b-8hlg5   0/1     Completed           0          28m
nginx-deployment-68bcd9bb5b-8hlg5   0/1     Completed           0          28m

PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get pods -w
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-5874fd7657-cz5s5   1/1     Running   0          60s
nginx-deployment-5874fd7657-ph6bm   1/1     Running   0          101s
nginx-deployment-5874fd7657-vvj8q   1/1     Running   0          78s

PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get pods -o wide
NAME                                READY   STATUS    RESTARTS   AGE    IP           NODE                    NOMINATED NODE   READINESS GATES
nginx-deployment-5874fd7657-cz5s5   1/1     Running   0          80s    10.244.2.5   multi-cluster-worker    <none>           <none>
nginx-deployment-5874fd7657-ph6bm   1/1     Running   0          2m1s   10.244.3.2   multi-cluster-worker3   <none>           <none>
nginx-deployment-5874fd7657-vvj8q   1/1     Running   0          98s    10.244.1.2   multi-cluster-worker2   <none>           <none>
```
2. Node Affinity required

When the Node affinity required key value matched with the pod label, then it will create the pod in the node, else doesnot schedule the pod.
```powershell
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl apply -f node-affinity-required.yml
deployment.apps/nginx-deployment created
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-7947894b47-bqc89   0/1     Pending   0          8s
nginx-deployment-7947894b47-mwhf4   0/1     Pending   0          8s
nginx-deployment-7947894b47-n5vjp   0/1     Pending   0          8s

PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl describe pod nginx-deployment-7947894b47-bqc89
Name:             nginx-deployment-7947894b47-bqc89
Namespace:        default
Priority:         0
Service Account:  default
Node:             <none>
Labels:           app=nginx
                  pod-template-hash=7947894b47
Annotations:      <none>
Status:           Pending
IP:
IPs:              <none>
Controlled By:    ReplicaSet/nginx-deployment-7947894b47
Containers:
  nginx:
    Image:        nginx:1.14.2
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-j58gv (ro)
Conditions:
  Type           Status
  PodScheduled   False
Volumes:
  kube-api-access-j58gv:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  36s   default-scheduler  0/4 nodes are available: 1 node(s) had untolerated taint(s), 3 node(s) didn't match Pod's node affinity/selector. no new claims to deallocate, preemption: 0/4 nodes are available: 4 Preemption is not helpful for scheduling.

PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get nodes
NAME                          STATUS   ROLES           AGE   VERSION
multi-cluster-control-plane   Ready    control-plane   42m   v1.35.0
multi-cluster-worker          Ready    <none>          41m   v1.35.0
multi-cluster-worker2         Ready    <none>          41m   v1.35.0
multi-cluster-worker3         Ready    <none>          41m   v1.35.0

PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl edit node multi-cluster-worker3

 labels:
    beta.kubernetes.io/arch: amd64
    beta.kubernetes.io/os: linux
    kubernetes.io/arch: amd64
    kubernetes.io/hostname: multi-cluster-worker3
    kubernetes.io/os: linux
    foo: bar1


node/multi-cluster-worker3 edited
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl apply -f node-affinity-required.yml
deployment.apps/nginx-deployment created
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get pods                           
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-7947894b47-87vwm   1/1     Running   0          7s
```
nginx-deployment-7947894b47-8sdt8   1/1     Running   0          7s
nginx-deployment-7947894b47-mgs7b   1/1     Running   0          7s
