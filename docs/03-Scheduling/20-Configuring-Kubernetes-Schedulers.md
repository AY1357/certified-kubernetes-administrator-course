# Configuring Kubernetes Schedulers
  - Take me to [video Tutorial](https://kodekloud.com/topic/configuring-kubernetes-scheduler/)
  
In this section, we will take a look at configuring kubernetes schedulers.

![ks](../../images/ks.PNG)

## Scheduling Steps Overview:
- Scheduling Queue - All PODs that are "to be deployed" are entering the Scheduling Queue; The PODs are being "sorted" based on the "PriorityClassName" (under "spec") parameter.
- Filtering - NODES that cannot run the to be shceduled PODs are being filtered-out (whether it's due to low resources on the NODEs or etc..)
- Scoring - NODEs will get a "scoring" based on the "free-space" that will be left after deploying the POD. The more space left, the higher score that NODE will get.
- Binding - in this steps the POD will be "bind" to the chosen NODE.

The 1st POD to be deployed will be chosen based on the "Scheduling Queue", the NODE on which this POD will be deployed will be chosen based on the "Filtering" & "Scoring" of the NODE based on the POD requirements.

## Scheduling Plugins:
- Scheduling Queue - WIll use the "PrioritySort" plugin.
- Filtering - Will use the "NodeResourcesFit" plugin; NodeName plugin; NodeUnschedulable plugin; NodeResourcesFir ; TaintTolaration ; NodePorts ; NodeAffinity;
- Scoring - NodeResourcesFit plugin; ImageLocality plugin; NodeResourcesFit ; TaintTolaration ; NodeAffinity;
- Binding - DefaultBinder plugin;

## Extension Points:
These are the Extension Points to which the Plugins can be plugged-to
- Scheduling Queue - queueSort
- Filtering - preFilter ; filter ; postFilter
- Scoring - preScore ; score ; reserve
- Binding - permit ; preBind ; bind ; postBind

  
## References
- https://github.com/kubernetes/community/blob/master/contributors/devel/sig-scheduling/scheduler.md
- https://kubernetes.io/blog/2017/03/advanced-scheduling-in-kubernetes/
- https://jvns.ca/blog/2017/07/27/how-does-the-kubernetes-scheduler-work/
- https://stackoverflow.com/questions/28857993/how-does-kubernetes-scheduler-work

