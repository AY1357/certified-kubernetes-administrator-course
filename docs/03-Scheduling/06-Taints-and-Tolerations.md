# Taints and Tolerations
  - Take me to [Video Tutorial](https://kodekloud.com/topic/taints-and-tolerations-2/)
  
In this section, we will take a look at taints and tolerations.
- Pod to node relationship and how you can restrict what pods are placed on what nodes.

#### Taints and Tolerations are used to set restrictions on what pods can be scheduled on a node. 
- Only pods which are tolerant to the particular taint on a node will get scheduled on that node.

  ![tandt](../../images/tandt.PNG)
  
## Taints
- Use **`kubectl taint nodes`** command to taint a node.

  Syntax
  ```
  $ kubectl taint nodes <node-name> key=value:taint-effect
  ```
 
  Example
  ```
  $ kubectl taint nodes node1 app=blue:NoSchedule
  ```
  
- The taint effect defines what would happen to the pods if they do not tolerate the taint.
- There are 3 taint effects
  - **`NoSchedule`**
  - **`PreferNoSchedule`**
  - **`NoExecute`**
  
  ![tn](../../images/tn.PNG)
  
## Tolerations
   - Tolerations are added to pods by adding a **`tolerations`** section in pod definition.
     NOTE: The `tolerations` property that you see in the pod definition file below is equivelent to the command:
     `$ kubectl taint nodes node1 app=blue:NoSchedule` and takes the part `app=blue:NoSchedule` from the command mentioned.
     Keep in mind that the `values` in the `tolerations` property needs to be in double-quotes `"..."`.
     ```
     apiVersion: v1
     kind: Pod
     metadata:
      name: myapp-pod
     spec:
      containers:
      - name: nginx-container
        image: nginx
      tolerations:
      - key: "app"
        operator: "Equal"
        value: "blue"
        effect: "NoSchedule"
     ```
    
  ![tp](../../images/tp.PNG)

  NOTE: When the PODs are now created/updated with the new `tolerations`, they either not scheduled on the NODEs or evicted (PODs are destroyed) from the existing NODEs (depending on the effect set).



#### Taints and Tolerations do not tell the pod to go to a particular node. Instead, they tell the node to only accept pods with certain tolerations.
- To see this taint, run the below command
  ```
  $ kubectl describe node kubemaster |grep Taint
  ```
NOTE: This means that even if a POD is compatile in contexts of taitns and tolerations with NODE#1 it may be scheduled to be deployed on NODE#2.
 
 ![tntm](../../images/tntm.PNG)
  
     
#### K8s Reference Docs
- https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/

