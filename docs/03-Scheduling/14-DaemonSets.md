# DaemonSets
  - Take me to [Video Tutorial](https://kodekloud.com/topic/daemonsets/)

In this section, we will take a look at DaemonSets.

#### DaemonSets are like replicasets, as it helps in to deploy multiple instances of pod. But it runs one copy of your pod on each node in your cluster.

- When a new NODE is added to the Cluster, a replica of the POD is automatically added to that new NODE.
  And when the NODE is removed then the POD is automatically removed as well.
- The Deamonset ensures that 1 copy of the POD is always present in all NODEs in the cluster.
  
  ![ds](../../images/ds.PNG)
  
## DaemonSets - UseCases

  - One of the use cases is when we want to deploy a `Log Collector` on each NODE on the cluster to have a better monitoring of the cluster. In this case the `Deamonset` is perfect for this job as it will deploy exactly 1 replica of that `Log Collector` POD on each NODE in the cluster Automatically.
  - When changes in the cluster occur, like adding/removing NODEs, we will not have to worry about deploying the `Log Collector` as the `Deamonset` will take of this for us.

  ![ds-uc](../../images/ds-uc.PNG)


  - Earlier in this course we have learned that one of the PODs that HAS to be deployed on each NODE is the `kube-proxy`. That is one good use of `Deamonsets`.
    
  ![ds-uc-kp](../../images/ds-uc-kp.PNG)


  - Another use case is deploying the `weave-net` for networking solutions -> This will be discussed late rin the `Networking` section.
    
  ![ds-ucn](../../images/ds-ucn.PNG)
  
## DaemonSets - Definition
- Creating a DaemonSet is similar to the ReplicaSet creation process.
- For DaemonSets, we start with apiVersion, kind as **`DaemonSets`** instead of **`ReplicaSet`**, metadata and spec. See the definitions file below, the only difference in the `kind`:
  
`ReplicaSet` definition file:
  ```
  apiVersion: apps/v1
  kind: Replicaset
  metadata:
    name: monitoring-daemon
    labels:
      app: nginx
  spec:
    selector:
      matchLabels:
        app: monitoring-agent
    template:
      metadata:
       labels:
         app: monitoring-agent
      spec:
        containers:
        - name: monitoring-agent
          image: monitoring-agent
  ```

  `DaemonSets` definition file:
  ```
  apiVersion: apps/v1
  kind: DaemonSet
  metadata:
    name: monitoring-daemon
    labels:
      app: nginx
  spec:
    selector:
      matchLabels:
        app: monitoring-agent
    template:
      metadata:
       labels:
         app: monitoring-agent
      spec:
        containers:
        - name: monitoring-agent
          image: monitoring-agent
  ```
  ![dsd](../../images/dsd.PNG)
  
- To create a daemonset from a definition file
  ```
  $ kubectl create -f daemon-set-definition.yaml
  ```

## View DaemonSets
- To list daemonsets
  ```
  $ kubectl get daemonsets
  ```
- For more details of the daemonsets
  ```
  $ kubectl describe daemonsets monitoring-daemon
  ```
  ![ds1](../../images/ds1.PNG)
  
## How DaemonSets Works
- Until v/1.12 it worked as follows:
  We set on each POD set the `nodeName` property in it's sepcification before it is created, and when they are created they are automatically land on the respective NODEs (basically this is to bypass the `Scheduler` and get the POD placed on a NODE directly).

- FROM v/1.12 onwards the `Deamonset` uses the default `Scheduler` and `nodeAffinity` rules to schedule PODs on NODEs.
  
  ![ds2](../../images/ds2.PNG)

#### K8s Reference Docs
- https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/#writing-a-daemonset-spec
