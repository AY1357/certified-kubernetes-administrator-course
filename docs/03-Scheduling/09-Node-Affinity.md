# Node Affinity
  - Take me to the [Video Tutorial](https://kodekloud.com/topic/node-affinity-2/)
  
In this section, we will talk about "Node Affinity" feature in kubernetes.

#### The primary feature of Node Affinity is to ensure that the pods are hosted on particular nodes.
- With **`Node Selectors`** we cannot provide the advance expressions.
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   nodeSelector:
    size: Large
  ```
  ![ns-old](../../images/ns-old.PNG)
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: In
              values: 
              - Large
              - Medium
  ```
  ![na](../../images/na.PNG)

NOTE: The `operator: In` means to place the POD `In` the Node that is cmpatible with the requirements set in the `affinity.nodeAffinity` rules.
  
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: NotIn
              values: 
              - Small
  ```
  ![na1](../../images/na1.PNG)

NOTE: The `operator: NotIn` means that the Node Affinity will match the NODEs with the labels in which the `size` is **NOT** set to `small`.

  
  ```
  apiVersion: v1
  kind: Pod
  metadata:
   name: myapp-pod
  spec:
   containers:
   - name: data-processor
     image: data-processor
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
          nodeSelectorTerms:
          - matchExpressions:
            - key: size
              operator: Exists
  ```
  
  ![na2](../../images/na2.PNG)

NOTE: The `operator: Exists` means that the Node Affinity will **check** if the label `size` exists on the NODEs and we will not need the `values` section for that as it does **not** compare the `values`.

NOTE: More `operators` available in the documentation section.
  

## Node Affinity Types

NOTE: The Node Affinity Types defines the behaviour of the scheduler with the contexts of Node Affinity and the stages of the life-cycle of the PODs. The available types are:

- Available
  - requiredDuringSchedulingIgnoredDuringExecution
  - preferredDuringSchedulingIgnoredDuringExecution
- Planned
  - requiredDuringSchedulingRequiredDuringExecution
  - preferredDuringSchedulingRequiredDuringExecution
  
  ![nat](../../images/nat.PNG)
  
  
## Node Affinity Types States
NOTE: `DuringScheduling` (which is located in the affinity type keyword property) means that this rule is to be looked-at when the POD does not exists and is created for the first time. 

NOTE#2: The `Required` keyword -> the POD MUST have the rules mentioned in the affinity rules, if a match will not be found, then the POD will NOT be scheduled! ; The `Preferred` keyword means "do best-effort", but if match is not found in the affinity rules then the scheduler will simply will ignore these rules and will shcedule the POD on any available  NODE.

NOTE#3: The `DuringExecution` with value of `Ignored` means that PODs will comtinue to RUN and ANY change in the node affinity rules will NOT affect them once they are scheduled ; The `Required` value in the same keyword will have the scheduler evict (destroy) any POD that does not comply with the affinity rules even if this POD is already scheduled and is running.

  ![nats](../../images/nats.PNG)
  
  ![nats1](../../images/nats1.PNG)

  
#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/configure-pod-container/assign-pods-nodes-using-node-affinity/
- https://kubernetes.io/blog/2017/03/advanced-scheduling-in-kubernetes/
