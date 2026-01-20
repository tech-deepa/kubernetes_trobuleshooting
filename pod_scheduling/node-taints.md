```powershell
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get nodes
NAME                          STATUS   ROLES           AGE   VERSION
multi-cluster-control-plane   Ready    control-plane   42m   v1.35.0
multi-cluster-worker          Ready    <none>          41m   v1.35.0
multi-cluster-worker2         Ready    <none>          41m   v1.35.0
multi-cluster-worker3         Ready    <none>          41m   v1.35.0
error: the server doesn't have a resource type "multi-cluster-worker3"
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl edit node multi-cluster-worker3
node/multi-cluster-worker3 edited
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl apply -f node-affinity-required.yml
deployment.apps/nginx-deployment created
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-7947894b47-87vwm   1/1     Running   0          7s
nginx-deployment-7947894b47-8sdt8   1/1     Running   0          7s
nginx-deployment-7947894b47-mgs7b   1/1     Running   0          7s
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get pods
NAME                                READY   STATUS    RESTARTS   AGE
nginx-deployment-7947894b47-87vwm   1/1     Running   0          81m
nginx-deployment-7947894b47-8sdt8   1/1     Running   0          81m
nginx-deployment-7947894b47-mgs7b   1/1     Running   0          81m
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl delete deployment nginx-deployment
deployment.apps "nginx-deployment" deleted from default namespace
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get nodes   
NAME                          STATUS   ROLES           AGE    VERSION
multi-cluster-control-plane   Ready    control-plane   125m   v1.35.0
multi-cluster-worker          Ready    <none>          125m   v1.35.0
multi-cluster-worker2         Ready    <none>          125m   v1.35.0
multi-cluster-worker3         Ready    <none>          125m   v1.35.0
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl tain node multi-cluster-worker key1=value1:Nosechule
error: unknown command "tain" for "kubectl"

Did you mean this?
        drain
        taint
        wait
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl taint node multi-cluster-worker key1=value1:Nosechule
error: invalid taint effect: Nosechule, unsupported taint effect
See 'kubectl taint -h' for help and examples
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl taint nodes multi-cluster-worker key1=value1:Nosechule
error: invalid taint effect: Nosechule, unsupported taint effect
See 'kubectl taint -h' for help and examples
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl taint nodes multi-cluster-worker key1=value1:Nosechdule
error: invalid taint effect: Nosechdule, unsupported taint effect
See 'kubectl taint -h' for help and examples
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl taint nodes multi-cluster-worker key1=value1:Noschedule
error: invalid taint effect: Noschedule, unsupported taint effect
See 'kubectl taint -h' for help and examples
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl taint nodes multi-cluster-worker key1=value1:Noschedule
error: invalid taint effect: Noschedule, unsupported taint effect
See 'kubectl taint -h' for help and examples
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl taint nodes multi-cluster-worker key1=value1:NoSchedule
node/multi-cluster-worker tainted
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get nodes                                              
NAME                          STATUS   ROLES           AGE    VERSION
multi-cluster-control-plane   Ready    control-plane   128m   v1.35.0
multi-cluster-worker          Ready    <none>          128m   v1.35.0
multi-cluster-worker2         Ready    <none>          128m   v1.35.0
multi-cluster-worker3         Ready    <none>          128m   v1.35.0
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl taint nodes multi-cluster-worker2 key1=value1:NoSchedule
node/multi-cluster-worker2 tainted
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl apply -f deployment-sample.yml
deployment.apps/nginx-deployment created
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get pod -o wide
NAME                                READY   STATUS    RESTARTS   AGE   IP           NODE                    NOMINATED NODE   READINESS GATES
nginx-deployment-77bc6bd484-f5x4s   1/1     Running   0          8s    10.244.3.7   multi-cluster-worker3   <none>           <none>
nginx-deployment-77bc6bd484-nqlgm   1/1     Running   0          8s    10.244.3.6   multi-cluster-worker3   <none>           <none>
nginx-deployment-77bc6bd484-rc26s   1/1     Running   0          8s    10.244.3.8   multi-cluster-worker3   <none>           <none>

After adding toleration in the manifest file, it created the pod in the node that matches the taints label

PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl apply -f node-taint-toleration.yml
deployment.apps/nginx-deployment created
PS C:\Users\Dee\project\kubernetes_trobuleshooting\pod_scheduling> kubectl get pods -o wide
NAME                                READY   STATUS    RESTARTS   AGE   IP           NODE                    NOMINATED NODE   READINESS GATES
nginx-deployment-5cf545f787-9kkgc   1/1     Running   0          9s    10.244.2.6   multi-cluster-worker    <none>           <none>
```
nginx-deployment-5cf545f787-kd6px   1/1     Running   0          9s    10.244.1.3   multi-cluster-worker2   <none>           <none>
nginx-deployment-5cf545f787-xh7jq   1/1     Running   0          9s    10.244.3.9   multi-cluster-worker3   <none>           <none>
