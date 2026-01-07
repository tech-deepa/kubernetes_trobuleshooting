PS C:\Users\Dee\project\kubernetes_trobuleshooting\Imagepullbackoff\Invalid_image> kubectl get deployment 
E0107 18:47:35.845180 34936 memcache.go:265] "Unhandled Error" err="couldn't get current server API group list: Get \"https://azuredevops-dns-yefls0d8.hcp.centralindia.azmk8s.io:443/api?timeout=32s\": dial tcp: lookup azuredevops-dns-yefls0d8.hcp.centralindia.azmk8s.io: no such host"

PS C:\Users\Dee\project\kubernetes_trobuleshooting\Imagepullbackoff\Invalid_image> kubectl config use-context docker-desktop
Switched to context "docker-desktop".
PS C:\Users\Dee\project\kubernetes_trobuleshooting\Imagepullbackoff\Invalid_image> kubectl config current-context     
docker-desktop
PS C:\Users\Dee\project\kubernetes_trobuleshooting\Imagepullbackoff\Invalid_image> kubectl config get-contexts
CURRENT   NAME             CLUSTER          AUTHINFO                              NAMESPACE
          azuredevops      azuredevops      clusterUser_dee-prac-rg_azuredevops
*         docker-desktop   docker-desktop   docker-desktop
PS C:\Users\Dee\project\kubernetes_trobuleshooting\Imagepullbackoff\Invalid_image> kubectl config use-context docker-desktop
Switched to context "docker-desktop".

PS C:\Users\Dee\project\kubernetes_trobuleshooting\Imagepullbackoff\Invalid_image> kubectl get pods -w        
NAME                               READY   STATUS              RESTARTS   AGE
nginx-deployment-b7cdd6db8-sbwgp   0/1     ContainerCreating   0          3s
nginx-deployment-d556bf558-5bpbn   0/1     ErrImagePull        0          2m13s
nginx-deployment-d556bf558-cjlzr   0/1     ImagePullBackOff    0          2m13s
nginx-deployment-d556bf558-j78g8   0/1     ImagePullBackOff    0          2m13s
nginx-deployment-d556bf558-5bpbn   0/1     ImagePullBackOff    0          2m14s
nginx-deployment-b7cdd6db8-sbwgp   0/1     ErrImagePull        0          10s
nginx-deployment-d556bf558-cjlzr   0/1     ErrImagePull        0          2m26s
nginx-deployment-d556bf558-5bpbn   0/1     ImagePullBackOff    0          2m26s

after deleting the deployment:
we need to delete the pods also , as it does not reset the containerd and backoff timer
kubectl delete pod -l app=nginx
