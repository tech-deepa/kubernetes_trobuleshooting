Command crashLoopBackoff

```powershell
PS C:\Users\Dee\project\kubernetes_trobuleshooting\crashLoopBackoff> kubectl get pod
NAME                                READY   STATUS             RESTARTS     AGE
crash-deployment-77cd4fc6cb-s2hp5   0/1     CrashLoopBackOff   1 (4s ago)   7s
PS C:\Users\Dee\project\kubernetes_trobuleshooting\crashLoopBackoff> kubectl get pod -w
NAME                                READY   STATUS             RESTARTS     AGE
crash-deployment-77cd4fc6cb-s2hp5   0/1     CrashLoopBackOff   1 (8s ago)   11s
crash-deployment-77cd4fc6cb-s2hp5   0/1     CrashLoopBackOff   1 (17s ago)   20s
crash-deployment-77cd4fc6cb-s2hp5   0/1     Error              2 (18s ago)   21s
crash-deployment-77cd4fc6cb-s2hp5   0/1     CrashLoopBackOff   2 (13s ago)   33s
crash-deployment-77cd4fc6cb-s2hp5   0/1     Error              3 (25s ago)   45s
crash-deployment-77cd4fc6cb-s2hp5   0/1     CrashLoopBackOff   3 (16s ago)   60s
```

Liveness probe crashloopbackoff
```powershell
PS C:\Users\Dee\project\kubernetes_trobuleshooting\crashLoopBackoff> kubectl get pods -w
NAME                               READY   STATUS             RESTARTS      AGE
crash-deployment-557c987c4-scnrf   0/1     CrashLoopBackOff   1 (16s ago)   17s
crash-deployment-557c987c4-scnrf   0/1     CrashLoopBackOff   1 (17s ago)   18s
crash-deployment-557c987c4-scnrf   0/1     Error              2 (18s ago)   19s
crash-deployment-557c987c4-scnrf   0/1     CrashLoopBackOff   2 (2s ago)    20s
crash-deployment-557c987c4-scnrf   0/1     CrashLoopBackOff   2 (17s ago)   35s
crash-deployment-557c987c4-scnrf   0/1     CrashLoopBackOff   2 (28s ago)   46s
crash-deployment-557c987c4-scnrf   1/1     Running            3 (28s ago)   46s
crash-deployment-557c987c4-scnrf   0/1     Error              3 (29s ago)   47s
crash-deployment-557c987c4-scnrf   0/1     CrashLoopBackOff   3 (2s ago)    48s
```

OutOfMemory crashLoopbackoff
```powershell
PS C:\Users\Dee\project\kubernetes_trobuleshooting\crashLoopBackoff> kubectl get pods -w
NAME                                 READY   STATUS    RESTARTS   AGE
crashloop-example-6ccc5d6877-6sqzt   1/1     Running   1          76s
crashloop-example-6ccc5d6877-6sqzt   0/1     OOMKilled   1          98s
crashloop-example-6ccc5d6877-6sqzt   0/1     CrashLoopBackOff   1 (13s ago)   111s
crashloop-example-6ccc5d6877-6sqzt   1/1     Running            2             114s
crashloop-example-6ccc5d6877-6sqzt   0/1     OOMKilled          2             2m16s
crashloop-example-6ccc5d6877-6sqzt   0/1     CrashLoopBackOff   2 (11s ago)   2m27s
```
-----------------------------
