
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
5. To delete Pod `kubectl delete pod apiV<podName>`
6. To get Yaml produced for Pod `kubectl run <podName> --image=<imageName> --dry-run=client -o yaml > pod-defn.yaml`
7. Update config for the pd `kubectl apply -f pod-defn.yaml`
8. Update just the pod image `kubectl set image pod <pod_name> <container_name>=<new_image_name>`

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
        app : my-app
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
5. Update the image `kubectl set image replicaset/<replicaset_name> <container_name>=<new_image_name>`
6. Get yaml config [ omit -n if it is default namespace] `kubectl get replicaset <replicaset_name> -n <namespace> -o yaml > replicaset.yaml`

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



### Deployments
1. Create deployment : `kubectl create -f deployment-def.yaml`
    1. `kubectl create -f deployment-def.yaml --record` : Allows K8s to record revision with cause of change
2. Get deployments: `kubectl get deployments`
3. Update :
    1. Change the file and run : `kubectl apply -f deployment-def.yaml`
    2. This allows change + run both : `kubectl edit deployment <deployment-name> --record`

4. Status check :
    1. Gives current status `kubectl rollout status deployment/<deployment-name>`
    2. Gives historical information on changes done `kubectl rollout history deployment/<deployment-name>`
5. Rollback: `kubectl rollout undo deployment/<deployment-name>`
  During Rollback, the revision history is overwritten by new revision history.
  Ex: If revision2 was for CMD-1 and revision3 for CMD-2, if we rollback from CMD2
  CMD-1, revision2 would be removedn  from history & revision4 with CMD-1 will be
  added to it

```YAML
apiVersion: apps/v1 # NOTE: Change in appversion
kind: Deployment
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


### Services

1. Services are of type NodePort, ClusterIP, LoadBalancers
2. To get the URL of the minikube service `minikube service <service-name> --url`

```YAML
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  selector:
    app: my-app
  type: NodePort
  ports:
    - protocol: TCP
      port: 8080       # The port on the NodePort
      targetPort: 80   # The port on the Pods
      nodePort: 30000  # The specific port on the node (optional, Kubernetes will allocate if not specified)

---
apiVersion: v1
kind: Service
metadata:
  name: my-clusterip-service
spec:
  selector:
    app: my-app
  type: ClusterIP
  ports:
    - protocol: TCP
      port: 80         # The port exposed by the service within the cluster
      targetPort: 8080 # The port on the Pods that the service will forward traffic to
---
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  selector:
    app: my-app
  type: LoadBalancer
  ports:
    - protocol: TCP
      port: 80         # The port exposed by the service externally
      targetPort: 8080 # The port on the Pods that the service will forward traffic to

```


### Demo Voting Application




### Config Maps

```YAML
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: production
  DB_HOST: db.example.com
  DB_PORT: "5432"
```
Injecting the data in the POD
```YAML
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-app-image:latest
      envFrom:
        - configMapRef:
            name: app-config
```

### Secrets

```YAML
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
type: Opaque
data:
  username: <base64-encoded-username>
  password: <base64-encoded-password>
```

Using Secrets
```YAML
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: my-container
      image: my-app-image:latest
      env:
        - name: DB_USERNAME
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: username
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: password
```
