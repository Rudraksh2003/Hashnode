---
title: "How to Use kubectl to SSH into Pods and Control Their Life Cycle"
seoTitle: "How to Use kubectl to SSH into Pods and Control Their Life Cycle"
seoDescription: "Learn how to SSH into Minikube and manage pods with kubectl. This tutorial covers the basics of pods in Kubernetes."
datePublished: Mon Jul 24 2023 18:42:39 GMT+0000 (Coordinated Universal Time)
cuid: clkh7toju000f09m92r8p5pp7
slug: how-to-use-kubectl-to-ssh-into-pods-and-control-their-life-cycle
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/4Mw7nkQDByk/upload/630de61d387cc53db32193aa8e26f8b0.jpeg
tags: kubernetes, devops, minikube, wemakedevs, kubernetes-pods

---

In Kubernetes, pods are the basic deployment unit. They are a collection of containers that are organized and handled as a single entity. The `kubectl` command-line tool can be used to generate and destroy pods.

We'll show you how to SSH Minikube and create and destroy pods in this blog article. We'll also go over pods and how to make an alias for the `kubectl` command.

**SSHing into Minikube**

To SSH into Minikube, you can use the following command:

```plaintext
kubectl ssh <pod-name>
```

For example, to SSH into a pod named `nginx`, you would use the following command:

```plaintext
kubectl ssh nginx
```

This will open a shell inside the pod. You can then use the `bash` command to interact with the pod.

**Creating and Destroying Pods**

To create a pod, you can use the following command:

```plaintext
kubectl create pod <pod-name>
```

For example, to create a pod named `nginx`, you would use the following command:

```plaintext
kubectl create pod nginx
```

This will create a pod that runs the Nginx container.

To destroy a pod, you can use the following command:

```plaintext
kubectl delete pod <pod-name>
```

For example, to destroy the pod named `nginx`, you would use the following command:

```plaintext
kubectl delete pod nginx
```

**How Pods Work**

Pods are ephemeral. This means that they are created and destroyed as needed. When you create a pod, Kubernetes assigns it a unique name and IP address. The pod is then placed on one of the nodes in the cluster.

Pods are made up of one or more containers. Containers are lightweight, isolated units of execution that share the same operating system kernel. Each container in a pod has its own filesystem, network, and process space.

Pods are managed by the Kubernetes scheduler. The scheduler is responsible for placing pods on nodes in the cluster. The scheduler takes into account the resources that are available on each node, as well as the requirements of the pods.

**Creating an Alias for the** `kubectl` **Command**

You can create an alias for the `kubectl` command to make it easier to use. To do this, add the following line to your `.bashrc` file:

```plaintext
alias k='kubectl'
```

After you save the `.bashrc` file, you can use the `k` command instead of the `kubectl` command.

**Seeing That Pods Are Running**

You can use the `kubectl get pods` command to see if pods are running. This command will list all of the pods in the cluster.

For example, to see that the `nginx` pod is running, you would use the following command:

```plaintext
kubectl get pods
```

This will output a list of all of the pods in the cluster. The output will include the name, status, and IP address of each pod.

I hope you found this blog post helpful. If you have any questions, please leave a comment below.

---

**<mark>If you find this article helpful, please consider sponsoring me for more in-depth articles and contributions. Your support helps me create even better content! 🙌</mark>*Thank you for reading!***