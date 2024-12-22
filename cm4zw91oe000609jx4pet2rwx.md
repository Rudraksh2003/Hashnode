---
title: "The Magic of Multi-Node Kubernetes Setup: Organizing Your Resources"
datePublished: Sun Dec 22 2024 17:40:25 GMT+0000 (Coordinated Universal Time)
cuid: cm4zw91oe000609jx4pet2rwx
slug: the-magic-of-multi-node-kubernetes-setup-organising-your-resources
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1734892741848/9c41c53f-5477-4c33-bd72-b831381201a7.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1734892759106/54a36868-5c48-495d-aff4-3294592f4491.png
tags: microservices, kubernetes, devops, node, containers, containerization, devops-articles, kubernetes-container, kubernetes-architecture

---

Imagine you have a basket full of diverse food items. If someone asks for the smallest fruit, you might have to dig through the entire basket to find it, which takes time. But if you organize the items into separate baskets—one for fruits, another for vegetables, and a third for pickles—you can easily locate what you need. This concept of organisation is the key to efficiency, and Kubernetes achieves a similar feat with nodes in a multi-node setup.

![scrum kanban GIF by Stoneham Press](https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExZnFqOG5yYnA0cW9hZG0xZW1pOTY2cGg5NTViMWNoOWZhOXo4Y3pzNCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/PbFp1FmxteXsc/giphy.gif align="left")

---

In Kubernetes, nodes are like the baskets, and they help organize your workloads (pods). For example, if you need to update a database pod or resolve an error, you don’t need to scan every node. Instead, you know exactly where to look. This magic of node separation not only simplifies management but also optimises resource allocation.

Let’s explore how to set up a multi-node Kubernetes cluster and label nodes for specific purposes like database, application, or dependent services.

### Setting Up a Multi-Node Kubernetes Cluster

To create a multi-node Kubernetes cluster, we’ll use Minikube. Minikube is a tool that makes it easy to run Kubernetes locally. Here’s how you can start a cluster with three nodes:

```plaintext
minikube start --nodes=3 --cpus=2 --memory=4g --driver=docker
```

* `--nodes=3`: Specifies the number of nodes in the cluster.
    
* `--cpus=2`: Allocates two CPUs per node.
    
* `--memory=4g`: Allocates 4 GB of memory per node.
    
* `--driver=docker`: Specifies Docker as the container runtime driver.
    

Once the cluster is up and running, you can verify the nodes using:

```plaintext
kubectl get nodes
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734888576135/6d421dfe-964d-4d5c-82ca-4109ef687809.png align="center")

### Labeling Nodes for Specific Roles

Labeling nodes allows you to assign specific roles to them. For instance, you can dedicate one node for the application, another for the database, and the third for dependent services. Here’s how you can label the nodes:

![Organise What Am I GIF by Caroline - The Happy Sensitive](https://media1.giphy.com/media/v1.Y2lkPTc5MGI3NjExYTd1aGp6YjUyMmo0bGUxMzUxYXR1bDZnaTJ5cXNwN3F4YXZ3Y2k3NiZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/CEcB02dadAY8E7tbs0/giphy.gif align="left")

1. Label the first node for the application:
    
    ```plaintext
    kubectl label nodes minikube type=student-api
    ```
    
2. Label the second node for the database:
    
    ```plaintext
    kubectl label nodes minikube-m02 type=database
    ```
    
3. Label the third node for dependent services:
    
    ```plaintext
    kubectl label nodes minikube-m03 role=dependent-services
    ```
    

You can confirm the labels by running:

```plaintext
kubectl get nodes --show-labels
```

or

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1734888646446/63486235-1ef3-47dc-8a41-bd0f2f2216e9.png align="center")

### Deploying Workloads to Specific Nodes

To deploy pods to specific nodes, use the `nodeSelector` field in your pod specifications. For example, if you want to deploy a pod to the database node:

```plaintext
apiVersion: v1
kind: Pod
metadata:
  name: database-pod
spec:
  containers:
  - name: database
    image: mysql:5.7
  nodeSelector:
    type: database
```

Similarly, you can create configurations for other nodes based on their labels.

### Benefits of Node Organization

1. **Simplified Management**: Knowing the specific role of each node makes it easier to manage and troubleshoot.
    
2. **Optimized Resource Allocation**: Resources are efficiently utilized as workloads are assigned to the appropriate nodes.
    
3. **Improved Performance**: Workloads can be isolated to reduce interference and improve performance.
    
4. **Scalability**: Adding new nodes for specific purposes becomes straightforward.
    

### Conclusion

A multi-node Kubernetes setup is a powerful way to organise and manage workloads. By labelling nodes based on their roles, you can streamline operations, improve performance, and reduce downtime. So, next time you need to locate or manage a specific pod, you’ll know exactly where to look—just like finding the smallest fruit in a well-organized basket.

---

Thank you for reading!