## YAML usage in Kubernetes
### Required Fields
Kubernates definition files always contain 4 top level  fields.

* **apiVersion** - Which version of the Kubernetes API you're using to create this object
* **kind** - What kind of object you want to create
* **metadata** - Data that helps uniquely identify the object, including a name string, UID, and optional namespace
* **spec** - What state you desire for the object

### PODs with YAML
![image](https://github.com/Mfarzana/docker-learning/blob/master/images/pod-with-yaml.jpg)

### References:
* https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/
* https://www.containerlabs.kubedaily.com/Kubernetes/fundamentals/Pod.html
* https://developer.ibm.com/technologies/containers/tutorials/yaml-basics-and-usage-in-kubernetes/
