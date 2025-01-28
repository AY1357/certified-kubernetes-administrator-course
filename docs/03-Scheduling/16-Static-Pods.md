# Static Pods 
  - Take me to [Video Tutorial](https://kodekloud.com/topic/static-pods/)
  
In this section, we will take a look at Static Pods

#### How do you provide a pod definition file to the kubelet without a kube-apiserver?
- You can configure the kubelet to read the pod definition files from a directory on the server designated to store information about pods.

## Configure Static Pod
- The designated directory can be any directory on the host and the location of that directory is passed in to the kubelet as an option while running the service.
  - The option is named as **`--pod-manifest-path`**.
  
  ![sp](../../images/sp.PNG)
  
## Another way to configure static pod 
- Instead of specifying the option directly in the **`kubelet.service`** file, you could provide a path to another config file using the config option, and define the directory path as staticPodPath in the file.

  ![sp1](../../images/sp1.PNG)

## View the static pods
- To view the static pods
  ```
  $ docker ps
  ```
  or:
  to view the PODs from ALL namespaces execute the below command and search for the PODs that in their NAME they have some legit words and not "random" characters like in: "kube-flannel-ds-ct7qn" (the "ds-ct7qn" part)
  ```
  $ kubectl get pods -A 
  ```
  another way to make sure of that is to execute the below command and look for "ownerReference" section, from this section look for the "kind", if it's set to "Node" then it's a static POD; For non-static PODs, the "kind" will be set to "ReplicaSet".
  ```
  kubectl get pod <POD_FULL_NAME> -n <NAMESPACE> -o yaml
  ```
  ![sp2](../../images/sp2.PNG)

#### The kubelet can create both kinds of pods - the static pods and the ones from the api server at the same time.

  ![sp3](../../images/sp3.PNG)

## Static Pods - Use Case

  ![sp4](../../images/sp4.PNG)
  
  ![sp5](../../images/sp5.PNG)
  
## Static Pods vs DaemonSets

   ![spvsds](../../images/spvsds.PNG)
  

#### K8s Reference Docs
- https://kubernetes.io/docs/tasks/configure-pod-container/static-pod/
