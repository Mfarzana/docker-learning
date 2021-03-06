![enter image description here](https://d33wubrfki0l68.cloudfront.net/26a177ede4d7b032362289c6fccd448fc4a91174/eb693/images/docs/container_evolution.svg)

## Why you need Kubernetes and what it can do?.
**Containers** are a good way to **bundle and run your applications**. In a production environment, you need to manage the containers that run the applications and ensure that there is no downtime. **For example**, if a container goes down, another container needs to start. Wouldn't it be easier if this behavior was handled by a system?

That's how Kubernetes comes to the rescue! Kubernetes provides you with a framework to run distributed systems resiliently. It takes care of scaling and failover for your application, provides deployment patterns, and more. 

## Kubernetes Components
- When you deploy Kubernetes, you get a cluster.
- A Kubernetes **cluster consists of a set of worker machines, called nodes,** that run containerized applications. Every cluster has at least one worker node.
- The worker node(s) host the Pods that are the components of the application workload. **The control plane manages the worker nodes and the Pods in the cluster**. 

![Components of Kubernetes](https://d33wubrfki0l68.cloudfront.net/7016517375d10c702489167e704dcb99e570df85/7bb53/images/docs/components-of-kubernetes.png)



**Cluster:**
The cluster is a collection of Nodes (computers) related to each other and work together.
**Pod:**
The Pod is nothing but one or more docker containers, sharing the same network card (having the same localhost).

**Replica**:

The replica is a clone of a pod, replicas are needed to distribute the load.

## References
- [https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/](https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/)
- [https://kubernetes.io/docs/concepts/overview/components/](https://kubernetes.io/docs/concepts/overview/components/)


- [https://medium.com/yld-blog/kubernetes-core-concepts-324ea7028c29](https://medium.com/yld-blog/kubernetes-core-concepts-324ea7028c29)
- [https://medium.com/tech-tajawal/understand-kubernetes-main-concepts-in-2-minutes-68270fa3ceb6](https://medium.com/tech-tajawal/understand-kubernetes-main-concepts-in-2-minutes-68270fa3ceb6)
- [https://medium.com/@prakashkumar0301/kubernetes-key-component-and-concept-68c18e21cb95#:~:text=Kubernetes%20is%20an%20open%2Dsource,managing%20containerized%20apps%20in%20production.&text=Instead%2C%20you%20tell%20K8s%20your,all%20about%20abstracting%20away%20complexity.](https://medium.com/@prakashkumar0301/kubernetes-key-component-and-concept-68c18e21cb95#:~:text=Kubernetes%20is%20an%20open%2Dsource,managing%20containerized%20apps%20in%20production.&text=Instead%2C%20you%20tell%20K8s%20your,all%20about%20abstracting%20away%20complexity.)
- [https://medium.com/easyread/step-by-step-introduction-to-basic-concept-of-kubernetes-e20383bdd118](https://medium.com/easyread/step-by-step-introduction-to-basic-concept-of-kubernetes-e20383bdd118)
