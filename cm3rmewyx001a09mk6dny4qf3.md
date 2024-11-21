---
title: "A Comprehensive Guide to Helm: From Beginner to Advanced"
datePublished: Thu Nov 21 2024 18:03:11 GMT+0000 (Coordinated Universal Time)
cuid: cm3rmewyx001a09mk6dny4qf3
slug: a-comprehensive-guide-to-helm-from-beginner-to-advanced
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1732211980174/4e7d3749-267f-4fd0-8ba5-1e999eb0f7f1.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1732212141345/0f18a4a6-8350-4966-bea8-19383ed08003.png
tags: kubernetes, devops, helm, vault, prometheus, grafana, 2articles1week, helmfile, hashicorp, helm-chart

---

Helm is often referred to as the "package manager for Kubernetes," offering a streamlined way to package, configure, and deploy Kubernetes applications. In this article, we'll dive deep into Helm, starting with the basics and progressing to advanced concepts. We'll also contrast manual deployments with Helm-based deployments to showcase its practical advantages.

---

## What is Helm?

Helm is an open-source tool that simplifies managing Kubernetes applications. It uses predefined templates, called **Helm charts**, to deploy complex applications easily and consistently.

### Why Use Helm?

* **Simplifies Complex Deployments**: Automates the creation of Kubernetes resources.
    
* **Reusable Configurations**: Shareable charts eliminate repetitive YAML writing.
    
* **Version Control and Rollback**: Tracks deployments and enables effortless rollbacks.
    
* **Centralized Configuration**: All settings are managed via a single file, `values.yaml`.
    

---

## Installing Prometheus and Grafana Without Helm

### The YAML Files for Manual Installation

To deploy **Prometheus** and **Grafana** manually, you'll need the following Kubernetes resource definitions:

---

### Prometheus Deployment

#### Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: prometheus
  labels:
    app: prometheus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: prometheus
  template:
    metadata:
      labels:
        app: prometheus
    spec:
      containers:
      - name: prometheus
        image: prom/prometheus:latest
        ports:
        - containerPort: 9090
        volumeMounts:
        - name: prometheus-config
          mountPath: /etc/prometheus/
      volumes:
      - name: prometheus-config
        configMap:
          name: prometheus-config
```

#### ConfigMap for Prometheus Configurations

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: prometheus-config
data:
  prometheus.yml: |
    global:
      scrape_interval: 15s
    scrape_configs:
      - job_name: "prometheus"
        static_configs:
          - targets: ["localhost:9090"]
```

#### Service YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: prometheus-service
  labels:
    app: prometheus
spec:
  selector:
    app: prometheus
  ports:
    - protocol: TCP
      port: 9090
      targetPort: 9090
  type: ClusterIP
```

---

### Grafana Deployment

#### Deployment YAML

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana
  labels:
    app: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana
        image: grafana/grafana:latest
        ports:
        - containerPort: 3000
```

#### ConfigMap for Grafana Configurations

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: grafana-config
data:
  grafana.ini: |
    [server]
    root_url = %(protocol)s://%(domain)s/
    serve_from_sub_path = true
```

#### Service YAML

```yaml
apiVersion: v1
kind: Service
metadata:
  name: grafana-service
  labels:
    app: grafana
spec:
  selector:
    app: grafana
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
  type: LoadBalancer
```

---

### Challenges of Manual Deployment

1. **Complexity**: Writing multiple YAML files for every resource.
    
2. **Error-Prone**: Small mistakes in configuration can lead to failures.
    
3. **Maintenance Overhead**: Updating configurations requires manual edits and redeployments.
    

---

### Using Helm for Prometheus and Grafana

With Helm, the entire deployment becomes effortless:

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/kube-prometheus-stack
helm install grafana grafana/grafana
```

### Advantages of Helm

* **Single Command Deployment**: No need to manage individual YAML files.
    
* **Centralized Configuration**: All settings managed in `values.yaml`.
    
* **Easier Updates**: Modify `values.yaml` and use `helm upgrade`.
    
* **Rollback Support**: Rollback with `helm rollback`.
    

---

## Installing HashiCorp Vault Without and With Helm

### Manual Deployment

To install HashiCorp Vault without Helm, you need multiple Kubernetes resources:

#### Deployment YAML for Vault

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: vault
  labels:
    app: vault
spec:
  replicas: 1
  selector:
    matchLabels:
      app: vault
  template:
    metadata:
      labels:
        app: vault
    spec:
      containers:
      - name: vault
        image: hashicorp/vault:latest
        ports:
        - containerPort: 8200
```

#### Service YAML for Vault

```yaml
apiVersion: v1
kind: Service
metadata:
  name: vault-service
  labels:
    app: vault
spec:
  selector:
    app: vault
  ports:
    - protocol: TCP
      port: 8200
      targetPort: 8200
  type: ClusterIP
```

### Helm-Based Deployment

```bash
helm repo add hashicorp https://helm.releases.hashicorp.com
helm repo update
helm install vault hashicorp/vault --set "server.ha.enabled=true"
```

**Helm Benefits**:

* Automated HA setup.
    
* Simplified customization with `values.yaml`.
    

---

## Helm Charts Simplify Kubernetes

A Helm chart is a collection of files that describe a set of Kubernetes resources. It is packaged in a directory that contains:

* **Chart.yaml**: Metadata about the chart.
    
* **templates/**: Contains Kubernetes YAML templates that define the actual resources.
    
* **values.yaml**: The default configuration for the chart, where you define values that can be overridden during deployment.
    

#### b. Helm Repository

A Helm repository is a collection of Helm charts. You can store and share your Helm charts in these repositories, which can be public (e.g., Helm Hub) or private.

---

### Installing Helm and Setting Up a Chart

#### a. Installing Helm

To get started with Helm, you first need to install it on your local machine or in your environment.

```
bashCopy codebrew install helm      # For macOS
apt-get install helm    # For Ubuntu
```

After installation, you can verify Helm is working by running:

```plaintext
bashCopy codehelm version
```

#### b. Creating a New Helm Chart

You can create a new Helm chart using the following command:

```plaintext
bashCopy codehelm create my-chart
```

This will generate a basic Helm chart structure for you to customize. The structure includes a `Chart.yaml`, a `values.yaml` file, and a sample `templates/` directory containing example Kubernetes resource templates.

---

### Combining Deployment, Service, and ConfigMap

In a traditional Kubernetes setup, you need to create and manage multiple YAML files separately for each resource. For example, if you’re deploying an application, you might need:

1. A **Deployment** file to define pods and containers.
    
2. A **Service** file to expose your application.
    
3. A **ConfigMap** file to store configuration data.
    

With Helm, these YAML files can be bundled into a single **chart**.

---

#### Example: Helm Chart Directory Structure

Here’s a typical Helm chart structure that bundles these components:

```plaintext
my-app/
  ├── charts/                 # Sub-charts (if any)
  ├── templates/              # Kubernetes manifests as templates
  │   ├── deployment.yaml     # Deployment template
  │   ├── service.yaml        # Service template
  │   ├── configmap.yaml      # ConfigMap template
  ├── Chart.yaml              # Chart metadata
  ├── values.yaml             # Centralized default values
  ├── README.md               # Chart documentation
```

---

#### Templates for Deployment, Service, and ConfigMap

1. **Deployment Template (**`templates/deployment.yaml`)
    

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ .Values.appName }}
  labels:
    app: {{ .Values.appName }}
spec:
  replicas: {{ .Values.replicas }}
  selector:
    matchLabels:
      app: {{ .Values.appName }}
  template:
    metadata:
      labels:
        app: {{ .Values.appName }}
    spec:
      containers:
      - name: {{ .Values.containerName }}
        image: {{ .Values.image.repository }}:{{ .Values.image.tag }}
        ports:
        - containerPort: {{ .Values.containerPort }}
```

2. **Service Template (**`templates/service.yaml`)
    

```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Values.service.name }}
spec:
  type: {{ .Values.service.type }}
  ports:
  - port: {{ .Values.service.port }}
    targetPort: {{ .Values.service.targetPort }}
  selector:
    app: {{ .Values.appName }}
```

3. **ConfigMap Template (**`templates/configmap.yaml`)
    

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: {{ .Values.configmap.name }}
data:
  app-settings: |
    logLevel: {{ .Values.configmap.logLevel }}
    databaseURL: {{ .Values.configmap.databaseURL }}
```

---

### Centralized Customization with `values.yaml`

The `values.yaml` file is the heart of Helm’s flexibility. It allows you to define default values that can be overridden when deploying the chart. Here’s an example:

#### `values.yaml`

```yaml
appName: my-app
replicas: 2

image:
  repository: my-app-image
  tag: latest

containerName: my-app-container
containerPort: 8080

service:
  name: my-app-service
  type: ClusterIP
  port: 8080
  targetPort: 8080

configmap:
  name: my-app-config
  logLevel: debug
  databaseURL: jdbc://db-service:5432/mydb
```

---

### How Helm Combines and Simplifies

1. **Templates with Placeholders**:  
    Each YAML template uses placeholders (e.g., `{{ .Values.appName }}`) that dynamically pull values from `values.yaml`.
    
2. **Customization at Deployment Time**:  
    While the `values.yaml` file provides defaults, you can override these values when deploying the chart:
    
    ```bash
    helm install my-app ./my-app --set replicas=3,image.tag=v2
    ```
    
3. **Single Command Deployment**:  
    Instead of applying each YAML file manually, you deploy the entire application with:
    
    ```bash
    helm install my-app ./my-app
    ```
    

---

### Benefits of Combining Resources into a Single Chart

1. **Reusable Across Environments**:  
    A single chart can be used for development, staging, and production by overriding values specific to each environment.
    
2. **Reduced Errors**:  
    Helm automatically handles dependencies and ensures all resources are deployed correctly.
    
3. **Easier Maintenance**:  
    Centralized configuration in `values.yaml` makes updates straightforward. For instance, changing the image tag or scaling replicas is as simple as updating the file.
    
4. **Collaboration and Sharing**:  
    Helm charts are self-contained, making them easy to share across teams or the open-source community.
    

---

### Real-World Example: Deploying an Application

Imagine deploying an app with a Helm chart. Here’s the workflow:

1. Write the templates for Deployment, Service, and ConfigMap.
    
2. Define the default settings in `values.yaml`.
    
3. Deploy the app with one command:
    
    ```bash
    helm install my-app ./my-app
    ```
    
4. Update or scale by modifying `values.yaml` or using `--set` flags:
    
    ```bash
    helm upgrade my-app ./my-app --set replicas=4
    ```
    

---

## Helmfile

### What is Helmfile?

While **Helm** simplifies the management and deployment of Kubernetes applications by using Helm charts, **Helmfile** takes it a step further by managing multiple Helm releases and providing a way to define complex Helm deployments in a declarative way.

Helmfile is an open-source tool that helps manage **Helm charts** with better organization, especially when you're working with multiple environments and need to manage multiple releases (i.e., instances of Helm charts) simultaneously.

In simple terms, Helmfile helps you:

* Organize and manage multiple Helm charts and releases.
    
* Define Helm chart configurations in a single declarative file.
    
* Enable a structured, repeatable deployment of charts to multiple environments or clusters.
    
* Provide environment-specific configurations to handle different scenarios.
    

---

### Key Concepts in Helmfile

Before diving deeper into how Helmfile works, it's important to understand some of its core concepts.

#### a. **Helmfile** (the file)

A Helmfile is a configuration file (typically named `helmfile.yaml`) that contains the **declarative configuration** for the Helm releases you want to deploy. It defines:

* Which Helm charts to deploy.
    
* What values to pass to the charts.
    
* Where the charts should be installed.
    
* The specific namespaces and release names.
    

Helmfile allows you to automate the entire process of managing multiple Helm releases across various clusters with a single command.

#### b. **Releases**

A Helm release is an instance of a Helm chart deployed to a Kubernetes cluster. In Helmfile, you can define multiple releases, each with its own configurations, in the `helmfile.yaml`.

#### c. **Environments**

Helmfile supports environment-specific configurations. This means you can define different settings for different environments, such as **development**, **staging**, and **production**. This allows for flexibility and ensures that deployments are customized based on the target environment.

---

### Installing and Setting Up Helmfile

#### a. **Installation**

You can install Helmfile by running:

```bash
brew install helmfile     # For macOS
apt-get install helmfile   # For Ubuntu
```

Alternatively, you can download the latest release from the [Helmfile GitHub Releases page](https://github.com/roboll/helmfile/releases).

After installation, verify that Helmfile is installed properly by running:

```bash
helmfile --version
```

#### b. **Creating a Helmfile**

Once Helmfile is installed, you can create a `helmfile.yaml` to define your Helm releases. Here’s an example:

```yaml
repositories:
  - name: stable
    url: https://charts.helm.sh/stable

releases:
  - name: prometheus
    namespace: monitoring
    chart: stable/prometheus
    values:
      - values.yaml

  - name: grafana
    namespace: monitoring
    chart: stable/grafana
    values:
      - values.yaml
```

---

### Structure of a Helmfile

A Helmfile typically has the following structure:

```yaml
helmfile.yaml
  ├── repositories:         # Helm chart repositories
  │   ├── name: <repo-name> # The name of the repository
  │   └── url: <repo-url>   # The URL where the charts are hosted
  ├── releases:             # Helm chart releases
  │   ├── name: <release-name>   # Release name (this is the instance of a chart)
  │   ├── namespace: <namespace> # Kubernetes namespace where the release will be deployed
  │   ├── chart: <chart-name>    # The name of the Helm chart
  │   ├── values: <values-file>  # The values.yaml file to configure the release
  │   └── other properties      # Other configurations, like environment-specific values
  └── environments:           # Optional section for managing environments
```

#### a. **repositories**

The `repositories` section defines where Helm can find the Helm charts you want to deploy. Each repository is given a name and a URL.

#### b. **releases**

The `releases` section is the heart of the Helmfile configuration. Each release represents an instance of a chart that will be installed into a Kubernetes cluster. You can specify multiple releases, each with its own configuration.

#### c. **environments (Optional)**

The `environments` section helps you define environment-specific configurations. This allows you to manage different settings based on the target environment (e.g., development, staging, production). For instance:

```yaml
environments:
  prod:
    values:
      - prod-values.yaml
  dev:
    values:
      - dev-values.yaml
```

---

### Using Helmfile for Multi-Release and Multi-Environment Management

Helmfile shines when you need to manage multiple Helm releases across different environments. You can organize your Helm releases in a way that makes it easy to deploy the same set of charts across different environments with different configurations.

#### Example: Deploying Prometheus and Grafana in Multiple Environments

Here's an example `helmfile.yaml` where **Prometheus** and **Grafana** are deployed in **both the development and production environments** with different configurations.

```yaml
repositories:
  - name: stable
    url: https://charts.helm.sh/stable

releases:
  - name: prometheus
    namespace: monitoring
    chart: stable/prometheus
    values:
      - prometheus-values.yaml

  - name: grafana
    namespace: monitoring
    chart: stable/grafana
    values:
      - grafana-values.yaml

environments:
  dev:
    values:
      - dev-values.yaml
  prod:
    values:
      - prod-values.yaml
```

In this example, we’re managing **two releases (Prometheus and Grafana)**. The **values.yaml** files contain the specific settings for each environment. The `helmfile` command allows you to deploy these releases to both **development** and **production** with the correct configuration by simply switching environments:

```bash
helmfile -e dev apply      # Deploy to the development environment
helmfile -e prod apply     # Deploy to the production environment
```

---

### Benefits of Using Helmfile

Using **Helmfile** offers several advantages, especially for larger and more complex Kubernetes applications:

1. **Declarative Approach**: Helmfile provides a clear, version-controlled declaration of Helm releases, making it easy to track changes over time.
    
2. **Simplified Multi-Cluster/Environment Management**: You can manage Helm releases across multiple clusters or environments with environment-specific values.
    
3. **Reusability**: Like Helm charts, Helmfile configurations are reusable, allowing you to quickly replicate your setup in other clusters or environments.
    
4. **Centralized Management**: Helmfile centralizes all Helm chart deployment configurations, making it easier to manage large numbers of releases.
    
5. **Consistency**: Helmfile helps ensure that your releases are deployed consistently across different environments, reducing the risk of discrepancies.
    

---

### Deploying with Helmfile

To deploy your Helmfile configuration, you can use the `helmfile` command. The basic command structure is:

```bash
helmfile apply     # Deploys the Helm releases defined in the helmfile.yaml
```

You can also use `helmfile sync` to synchronize your Helm releases with the state defined in the Helmfile:

```bash
helmfile sync    # Syncs releases, ensuring the Helm releases match the Helmfile configuration
```

---

## Conclusion

Helm revolutionizes Kubernetes application management by reducing complexity, increasing consistency, and enabling automation. By comparing manual and Helm-based deployments, it’s clear that Helm is a must-have tool for Kubernetes practitioners.

Dive into Helm today to elevate your Kubernetes deployment workflows!