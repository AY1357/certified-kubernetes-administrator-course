# Manual Scheduling
  - Take me to [Video Tutorial](https://kodekloud.com/topic/manual-scheduling/)
  
In this section, we will take a look at **`Manually Scheduling`** a **`POD`** on a node.

## How Scheduling Works
- What do you do when you do not have a scheduler in your cluster?
  - Every POD has a field called NodeName that by default is not set. You don't typically specify this field when you create the manifest file, kubernetes adds it automatically.
  - The `Scheduler` goes through all the PODs and looks for those who do not have the `nodeName` property set. The PODs definition files (manifest files) that do NOT have this property are the candidates for `Scheduling`. It then identifies the right node for the POD prior running the `scheduling` algorithm. Once identified it schedules the POD on the node by setting the nodeName property to the name of the node by creating a binding object.  
    ```
    apiVersion: v1
    kind: Pod
    metadata:
     name: nginx
     labels:
      name: nginx
    spec:
     containers:
     - name: nginx
       image: nginx
       ports:
       - containerPort: 8080
     nodeName: node02
    ```
    ![sc1](../../images/sc1.png)
    
## No Scheduler
  - In case there's no `Scheduler` to monitor and schedule NODEs what happens? 
  - The PODs will continue being in the PENDING state. 

    What we can do about it? 
  - You can manually assign pods to node itself. Well without a scheduler, we can either: 
    Schedule pod to set nodeName property in your pod definition file while creating a pod. 
    NOTE: The above is available only when doing this in the creation POD step! 
    
    ![sc2](../../images/sc2.PNG)

    But what can we do in case the POD already exists? 
    We can use another way -> use the `binding-object` and set a POST request to the PODs `binding-api`, thus "mimicking" what the actual `scheduler` does! 
    In the `binding-object` we need to specify the `target-node` with the name of the `node` that we want to scheduler the POD on:
    
  - The YAML of the BINDING-OBJECT:
    ```
    apiVersion: v1
    kind: Binding
    metadata:
      name: nginx
    target:
      apiVersion: v1
      kind: Node
      name: node02
    ```
  - The POST command format that we need to send to PODs `binding-api`: 
    `Curl –header "Content-Type:application/json" --request POST –date '{apiVersion":"v1", "kind": "Binding" …}' `
  - The Address Format of POST request: `http://$SERVER/api/v1/namespaces/default/pods/$PODNAME/binding/`
  
  - The YAML of the POD:
    ```
    apiVersion: v1
    kind: Pod
    metadata:
     name: nginx
     labels:
      name: nginx
    spec:
     containers:
     - name: nginx
       image: nginx
       ports:
       - containerPort: 8080
    ```
    ![sc3](../../images/sc3.PNG)

  - NOTE: We cannot move a POD from one system/node to another system/node, so in that case we can do the following command: 
    `Kubectl replace –force –f nginx.yaml`

    
K8s Reference Docs:
- https://kubernetes.io/docs/reference/using-api/api-concepts/
- https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/#nodename
    
    
   
