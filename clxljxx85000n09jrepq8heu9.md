---
title: "Objects in Kubernetes"
datePublished: Wed Jun 19 2024 08:09:08 GMT+0000 (Coordinated Universal Time)
cuid: clxljxx85000n09jrepq8heu9
slug: objects-in-kubernetes
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1721028253517/81b66121-3d2a-411b-93c2-96e400941bac.avif
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1718784507460/18ef239b-95a6-4896-8aa6-693d40618de5.avif
tags: kubernetes, devops, kubernetes-container, kubernetes-architecture

---

### Understanding and Managing Kubernetes Objects

Kubernetes objects are fundamental components of the Kubernetes system, representing the desired state of your cluster. These objects provide a way to specify the containerized applications running in the cluster, the resources available to these applications, and the policies governing their behavior, such as restart policies, upgrades, and fault-tolerance.

#### Key Concepts

1. **Persistent Entities**: Kubernetes objects are persistent entities that the Kubernetes system maintains to ensure they exist as specified.
    
2. **Record of Intent**: By creating an object, you declare your desired cluster state, which Kubernetes continuously works to match with the actual state.
    
3. **API Interaction**: Interaction with Kubernetes objects is primarily through the Kubernetes API, either directly or using tools like `kubectl`.
    

#### Object Spec and Status

* **Spec**: The desired state of the object. You define this when creating the object.
    
* **Status**: The current state of the object, continuously updated by Kubernetes to match the spec.
    

For example, a Deployment object can specify the desired number of application replicas, and Kubernetes ensures the actual number matches the desired count.

#### Creating a Kubernetes Object

To create a Kubernetes object, you need to define its spec and some metadata in a manifest file, typically in YAML format. Here’s a basic example for a Deployment:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

You can create this object using `kubectl apply -f deployment.yaml`.

#### Required Fields in a Manifest

1. **apiVersion**: The API version for the object.
    
2. **kind**: The type of object.
    
3. **metadata**: Includes the name and namespace of the object.
    
4. **spec**: Defines the desired state of the object, with specific fields varying by object type.
    

#### Field Validation

Starting with Kubernetes v1.25, server-side field validation ensures that unrecognized or duplicate fields are detected. The `--validate` flag in `kubectl` sets the validation level, with options being:

* **Strict**: Errors on validation failure.
    
* **Warn**: Warnings on validation failure.
    
* **Ignore**: No validation performed.
    

This validation ensures that your configurations are correct before applying them to the cluster.

### Kubernetes Object Management

The `kubectl` command-line tool supports various techniques for creating and managing Kubernetes objects. Each technique is suited for different environments and purposes. This section provides an overview of these management approaches, their use cases, and their trade-offs.

#### Management Techniques

**Warning**: Use only one technique for managing a particular Kubernetes object to avoid undefined behavior.

| Management Technique | Operates on | Recommended Environment | Supported Writers | Learning Curve |
| --- | --- | --- | --- | --- |
| Imperative commands | Live objects | Development projects | 1+ | Lowest |
| Imperative object configuration | Individual files | Production projects | 1 | Moderate |
| Declarative object configuration | Directories of files | Production projects | 1+ | Highest |

#### Imperative Commands

* **Description**: Directly operate on live objects in a cluster using commands with arguments or flags.
    
* **Use Case**: Best for getting started or running one-off tasks.
    
* **Example**: `kubectl create deployment nginx --image nginx`
    

**Advantages**:

* Simple, single-step actions.
    
* Easy to execute without prior configuration.
    

**Disadvantages**:

* No change history or audit trail.
    
* Lack of integration with review processes.
    
* No templates for new objects.
    

#### Imperative Object Configuration

* **Description**: Use commands specifying operations (create, replace, etc.) with at least one YAML or JSON configuration file.
    
* **Use Case**: Suitable for production projects where object configurations need to be stored and reviewed.
    
* **Example**: `kubectl create -f nginx.yaml`
    

**Advantages**:

* Configurations can be stored in version control systems.
    
* Supports change reviews and audit trails.
    
* Provides templates for creating new objects.
    

**Disadvantages**:

* Requires understanding of the object schema.
    
* Involves additional steps to write configuration files.
    
* Configuration must be kept in sync with live objects to avoid data loss.
    

#### Declarative Object Configuration

* **Description**: Operate on local configuration files without specifying operations; `kubectl` automatically detects and applies necessary changes.
    
* **Use Case**: Ideal for managing directories of configuration files in production environments.
    
* **Example**: `kubectl apply -f configs/`
    

**Advantages**:

* Retains changes made directly to live objects.
    
* Supports operations on directories and automatic operation type detection.
    

**Disadvantages**:

* More complex to debug unexpected results.
    
* Partial updates using diffs can create complex merge and patch operations.
    

### Conclusion

Understanding and managing Kubernetes objects is crucial for maintaining the desired state of your cluster. By utilizing the Kubernetes API and tools like this `kubectl`, you can effectively manage the lifecycle of your containerized applications, ensuring reliability and scalability in your Kubernetes environment. Choosing the right management technique depends on your project's requirements and environment, balancing simplicity and control to maintain robust and scalable applications.