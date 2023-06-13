
# Kubernetes for beginner course by KodeKloud


## Basic Kubernetes commands


### Nodes

1. To get the information on nodes : `kubectl get nodes`
2. To run the node with the name  :  `kubectl run <node-name>`
3. To view cluster-info : `kubectl cluster-info`


### PODs

1. To view pods `kubectl get pods`, use `-o wide` to get more info
2. To view detailed info `kubectl describe pod <podname>`
3. To run the pod with node `kubectl run <podname> --image=<imgaeName>`
4. Create Pod using config file `kubectl create -f pod-defn.yaml`
5. To delete Pod `kubectl delete <podName>`
6. To get Yaml produced for Pod `kubectl run <podName> --imgae=<imageName> --dry-run=client -o yaml > pod-defn.yaml`

Config file for POD

```YAML
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
  labels: # labels can have custom fields
    app : my-app
    type: front-end
spec:
  containers:
    - name : nginx-c
      image: nginx

```

### Replication Controller

1. Create controller `kubectl create -f replication-controller.yaml`
2. Get controller : `kubectl get replicationcontroller`

Config file for Replication Controller

```YAML
apiVersion: v1
kind: ReplicationController
metadata:
  name: my-rc
  labels: # labels can have custom fields
    app : my-app
    type: front-end
spec:
  template:
    metadata:
      name: my-pod
      labels: # labels can have custom fields
        - app : my-app
          type: front-end
    spec:
      containers:
        - name : nginx-c
          image: nginx
  replicas: 3 # no of replicas to be maintained by controller

```


### Replicat Set

1. Create Replica Set `kubectl create -f replicaset-defn.yaml`
2. Get `kubectl get replicaset`
3. Delete `kubectl delete replicaset <replicasetName>`
4. Scale Replica set
  1. Update value of the field replicas and then run the command `kubectl replace -f replicaset-defn.yaml`
  2. OR Run the command with updated value `kubectl scale --replicas=6 -f replicaset-defn.yaml`
  3. OR `kubectl scale replicaset <replicasetName> --replicas=5`
  3. OR (NOT RECOMMENDED WAY) Edit using replicaset name `kubectl edit replicaset <replicasetName>`

Config file for Replica Set  
```YAML
apiVersion: apps/v1 # NOTE: Change in appversion
kind: ReplicaSet
metadata:
  name: my-rs
  labels: # labels can have custom fields
    app : my-app
    type: front-end
spec:
  template:
    metadata:
      name: my-pod
      labels: # labels can have custom fields
        app : my-app
        type: front-end
    spec:
      containers:
        - name : nginx-c
          image: nginx
  replicas: 3 # no of replicas to be maintained by controller
  selector:
    matchLabels:
      type: front-end # This is needed to filter the running pods to be controlled by Replica Set
```
