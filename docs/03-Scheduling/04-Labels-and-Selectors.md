# Labels and Selectors
  - Take me to [Video Tutorial](https://kodekloud.com/topic/labels-and-selectors/)
  
In this section, we will take a look at **`Labels and Selectors`**

#### Labels and Selectors are standard methods to group things together.
  
#### Labels are properties attached to each item.
Example from below, we have attached to each `Object` labels and their values, like the cat has the labels `Class: Mammal` ; `Kind: Domestic` ; `Color: Green`.

  ![labels-ckc](../../images/labels-ckc.PNG)
  
#### Selectors help you to filter these items By specifiying what we are looking for, for example: **`Class = Mammal & Color = Green`**, and we will get the following result:
 
  ![sl](../../images/sl.PNG)
  
How are labels and selectors are used in kubernetes?
- We have created different types of objects in kubernetes such as **`PODs`**, **`ReplicaSets`**, **`Deployments`** etc. With time we might find ourselves with a Cluster with many Objects and we will need to filter themn my application type, or by Object type, functionallity and etc... whatever it will be, we will be able to filter them using Labels & Selectors.
  
  ![ls](../../images/ls.PNG)

For each Object attach labels as per your needs, like `app` , `function`, etc...

How do you specify labels?
Under the `labels` section, sepcify the label name and it's value (key-value format), see example below:
   ```
    apiVersion: v1
    kind: Pod
    metadata:
     name: simple-webapp
     labels:
       app: App1
       function: Front-end
    spec:
     containers:
     - name: simple-webapp
       image: simple-webapp
       ports:
       - containerPort: 8080
   ```
 ![lpod](../../images/lpod.PNG)
 
Once the pod is created, to select the pod with labels run the below command
```
$ kubectl get pods --selector app=App1
```

Kubernetes uses labels to connect different objects together, for example, to create a `replicaSet` that consisting of 3 different PODs, we first labels the POD definition file (see below).

NOTE: The `labels` under the `template` section are the labels configured on the PODs ; The `labels` section at the top-most (it's the `labels` section that belongs to the `metadata` section under the `kind`) belong to the `replicaSet` itself.
   ```
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: simple-webapp
      labels:
        app: App1
        function: Front-end
    spec:
     replicas: 3
     selector:
       matchLabels:
        app: App1
     template:
       metadata:
         labels:
           app: App1
           function: Front-end
       spec:
         containers:
         - name: simple-webapp
           image: simple-webapp   
   ```
In order to connect the `replicaSet` to the POD, we configured the `selector` field under the `replicaSet` specification to match the `labels` that we have defind on the POD (See the information stated under the `matchLabels` section that it matches on of the `labels` that were specified under the `template` section), a single label will do if it matches the correctly. However, if you think that other PODs might match as well and we want to have a more explicit match then also specify the second label as well so that the right POD will be matched.

  ![lrs](../../images/lrs.PNG)

For services, it works in the same way, the `selector` section has the `labels` that should match the `POD`'s `labels` in the `template` of the replicaset-definition.yaml file.
 
      ```
      apiVersion: v1
      kind: Service
      metadata:
       name: my-service
      spec:
       selector:
         app: App1
       ports:
       - protocol: TCP
         port: 80
         targetPort: 9376 
       ```
  ![lrs1](../../images/lrs1.PNG)
  
## Annotations
- While labels and selectors are used to group objects, annotations are used to record other details for informative purpose.
    ```
    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: simple-webapp
      labels:
        app: App1
        function: Front-end
      annotations:
         buildversion: 1.34
    spec:
     replicas: 3
     selector:
       matchLabels:
        app: App1
    template:
      metadata:
        labels:
          app: App1
          function: Front-end
      spec:
        containers:
        - name: simple-webapp
          image: simple-webapp   
    ```
  ![annotations](../../images/annotations.PNG)

K8s Reference Docs:
- https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/
