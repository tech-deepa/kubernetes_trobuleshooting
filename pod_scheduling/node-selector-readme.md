PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl apply -f node-selector.yml
deployment.apps/nginx-deployment created
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get pods -w
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-68bcd9bb5b-8hlg5   0/1     Pending   0          6s
nginx-deployment-68bcd9bb5b-8r88h   0/1     Pending   0          6s
nginx-deployment-68bcd9bb5b-qdhvt   0/1     Pending   0          6s

PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl describe pod nginx-deployment-68bcd9bb5b-8hlg5
Name:             nginx-deployment-68bcd9bb5b-8hlg5
Namespace:        default
Priority:         0
Service Account:  default
Node:             <none>
Labels:           app=nginx
                  pod-template-hash=68bcd9bb5b
Annotations:      <none>
Status:           Pending
IP:
IPs:              <none>
Controlled By:    ReplicaSet/nginx-deployment-68bcd9bb5b
Containers:
  nginx:
    Image:        nginx:1.14.2
    Port:         80/TCP
    Host Port:    0/TCP
    Environment:  <none>
    Mounts:
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-vvm5r (ro)
Conditions:
  Type           Status
  PodScheduled   False
Volumes:
  kube-api-access-vvm5r:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    Optional:                false
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              foo=bar
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type     Reason            Age   From               Message
  ----     ------            ----  ----               -------
  Warning  FailedScheduling  4m5s  default-scheduler  0/4 nodes are available: 1 node(s) had untolerated taint(s), 3 node(s) didn't match Pod's node affinity/selector. no new claims to deallocate, preemption: 0/4 nodes are available: 4 Preemption is not helpful for scheduling.


  PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl edit node multi-cluster-worker
node/multi-cluster-worker edited

// here add the label in the node label section matching the nodeSelector label in the pod.

PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl apply -f node-selector.yml
deployment.apps/nginx-deployment unchanged
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get pod  
NAME                                READY   STATUS              RESTARTS   AGE
nginx-deployment-68bcd9bb5b-8hlg5   0/1     ContainerCreating   0          10m
nginx-deployment-68bcd9bb5b-8r88h   0/1     ContainerCreating   0          10m
nginx-deployment-68bcd9bb5b-qdhvt   1/1     Running             0          10m
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get pod -w
NAME                                READY   STATUS              RESTARTS   AGE
nginx-deployment-68bcd9bb5b-8hlg5   0/1     ContainerCreating   0          10m
nginx-deployment-68bcd9bb5b-8r88h   1/1     Running             0          10m
nginx-deployment-68bcd9bb5b-qdhvt   1/1     Running             0          10m
nginx-deployment-68bcd9bb5b-8hlg5   1/1     Running             0          10m