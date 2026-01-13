The stateful manifest, is used where we need to create a statedful application.

Here the stateful will create the pod, only after the previous pod is created.
Example if you have three replicas.
The second replica is created only after the 1 pod is running state.

The status of pod
----------------------
Container creating
pending
running

1. The problem faced is the pod is pending state:

kubectl describe pod
>> unfound the pvc   // kubectl cannot the storage class mentioned in the yaml

So always we need to match the pvc are available in the yaml file

aws --> ebs, efs
minikube --> standard

2. when we delete the stateful, we need to delete the pvc explictly.

3. Statefulset --> PV ---> Storage class --> provider ---> persistance volume
