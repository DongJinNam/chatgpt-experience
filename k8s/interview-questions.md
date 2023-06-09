# k8s interview question

k8s interview question 관련 질문 & 응답 모음입니다.

### top 8 k8s interview questions for junior developer

```
Question 1: What is Kubernetes, and what are its key components?
Answer:
Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. Its key components include:

* Master Node: Controls the Kubernetes cluster and manages scheduling, scaling, and communication with worker nodes.
* Worker Nodes: Host the containers and execute tasks assigned by the master node.
* Pods: The smallest unit in Kubernetes that encapsulates one or more containers.
* Replication Controller: Ensures a desired number of pod replicas are running.
* Services: Provide network connectivity and load balancing to pods.
* Volumes: Provide persistent storage to pods.

Question 2: What is a Kubernetes Deployment?
Answer:
A Kubernetes Deployment is an object that defines how to create and update instances of an application. It manages the creation of replica pods and allows rolling updates and rollbacks. It provides declarative updates, scaling, and self-healing capabilities.

Question 3: What is a Kubernetes Namespace?
Answer:
A Kubernetes Namespace is a virtual cluster that provides a way to logically partition resources within a cluster. It allows multiple teams or projects to share the same physical cluster while maintaining isolation. Namespaces can be used to allocate resources, manage access control, and segregate applications.

Question 4: Explain the concept of a Kubernetes Service.
Answer:
A Kubernetes Service is an abstraction that defines a logical set of pods and provides network access to them. It enables load balancing across the pods, allowing external traffic to be routed to the appropriate pods. Services can be exposed internally within the cluster or externally through NodePorts, LoadBalancers, or Ingress controllers.

Question 5: How can you scale a Kubernetes Deployment?
Answer:
You can scale a Kubernetes Deployment using the following methods:

* Manually updating the replica count in the Deployment specification.
* Using the kubectl scale command to change the replica count.
* Implementing Horizontal Pod Autoscaling (HPA) to automatically scale based on CPU utilization or other metrics.

Question 6: Explain the concept of a Kubernetes ConfigMap.
Answer:
A Kubernetes ConfigMap is an API object used to store non-confidential configuration data in key-value pairs. It allows decoupling of configuration from container images, making it easier to manage and update configuration settings independently of the application.

Question 7: How does Kubernetes handle container restarts and failures?
Answer:
Kubernetes provides built-in self-healing capabilities. If a container fails or terminates, the kubelet running on the worker node detects it and restarts the container. If a node fails, the master node identifies the failure and reschedules the affected pods to other healthy nodes.

These questions and answers should give you a good starting point for a Kubernetes interview as a junior developer. It's important to study and gain practical experience with Kubernetes to deepen your understanding of its concepts and usage.
```

### 배포 전략

```
Rolling Updates:
* Description: Rolling updates deploy new versions gradually by creating new pods while terminating the old ones.
* Pros:
  * Minimizes downtime by gradually transitioning to the new version.
  * Provides easy rollback to the previous version in case of issues.
* Cons:
  * Requires additional resources to run both old and new versions simultaneously during the update.
  * The update process may take longer if there are many pods to update.

Blue-Green Deployment:
* Description: Blue-Green deployment involves running two identical environments (blue and green) simultaneously, with only one active at a time.
* Pros:
  * Enables zero-downtime deployments by switching traffic between the blue and green environments.
  * Provides an easy rollback mechanism by routing traffic back to the previous environment.
* Cons:
  * Requires more resources to maintain two active environments simultaneously.
  * Requires additional configuration for routing traffic between environments.

Canary Deployment:
* Description: Canary deployment gradually introduces a new version to a subset of users while monitoring its performance before rolling it out to the entire user base.
* Pros:
  * Allows testing the new version in a controlled manner with a limited number of users.
  * Provides early feedback and helps identify issues before impacting all users.
* Cons:
  * Requires careful monitoring and analysis to ensure the new version performs well.
  * Increased complexity in managing traffic routing and version control.
```
