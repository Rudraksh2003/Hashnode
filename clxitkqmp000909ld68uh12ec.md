---
title: "Developer's Journey: Implementing Docker and Kubernetes for a Flask Application"
seoTitle: "Transforming Flask Applications with Docker and Kubernetes"
seoDescription: "Deploy Fast Grow's AI tools using Docker and Kubernetes, with step-by-step solutions to ensure smooth and efficient deployment."
datePublished: Mon Jun 17 2024 10:15:30 GMT+0000 (Coordinated Universal Time)
cuid: clxitkqmp000909ld68uh12ec
slug: developers-journey-implementing-docker-and-kubernetes-for-a-flask-application
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/npxXWgQ33ZQ/upload/e60043b9944c2435ddb10fb5d6d96119.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1718618943392/88b78959-3e97-4fde-ac44-3642ab9bf902.jpeg
tags: docker, projects, practice, kubernetes, devops, containers, devops-practices

---

#### Introduction

As a developer working on a Flask-based application, I recently embarked on a journey to containerize my application using Docker and orchestrate it with Kubernetes. This journey was filled with challenges and learning opportunities, culminating in a successful implementation. Here’s a detailed account of the process, the issues encountered, and how they were resolved.

---

#### Project Overview: Fast Grow

**Fast Grow** is your one-stop destination for cutting-edge content analysis tools. At Fast Grow, we harness the power of advanced AI technology to provide two essential services:

1. **YouTube Comment Key Point Finder**: This revolutionary solution is designed for content creators and marketers seeking valuable insights from audience feedback. Simply input the YouTube video link or comment section, and our AI algorithm swiftly identifies and highlights key points, enabling you to grasp the essence of user opinions and sentiments with unparalleled efficiency.
    
2. **Data Summarizer**: Setting a new standard in data analysis, this tool is perfect for students, researchers, and professionals. Our intuitive platform allows you to upload any document and pose specific questions. The AI engine meticulously scans the content, extracts relevant information, and generates concise summaries tailored to your inquiry. Gone are the days of laborious manual reading; with Fast Grow, access to comprehensive data summaries is just a click away.
    

You can explore the project further at [Fast Grow on GitHub](https://github.com/Rudraksh2003/Fast-Grow).

---

#### Dockerizing the Flask Application

##### Initial Setup

I started by creating a Dockerfile to containerize my Flask application. The Dockerfile is essential for defining the environment in which the application runs.

**Dockerfile:**

```dockerfile
# Use an official Python runtime as a parent image
FROM python:3.8-slim

# Set the working directory in the container
WORKDIR /app

# Copy the requirements file into the container
COPY requirements.txt .

# Install any needed packages specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the working directory contents into the container
COPY . .

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Run app.py when the container launches
CMD ["python", "app.py"]
```

##### Encountering and Resolving the Build Error

When I tried to build the Docker image using `docker build -t fast-grow .`, I encountered an error indicating that the `requirements.txt` file was not found. To resolve this issue, I created the `requirements.txt` file with the necessary dependencies and ensured it was in the same directory as the Dockerfile.

**requirements.txt:**

```plaintext
flask
requests
pandas
nltk
plotly
colorama
google-api-python-client
toml
streamlit
gunicorn
```

After adding the `requirements.txt` file, the Docker build process completed successfully.

---

#### Deploying the Application with Kubernetes

##### Creating Kubernetes Manifests

Next, I created Kubernetes manifests for deploying the application and its services.

**app-deployment.yaml:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: fast-grow-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: fast-grow-app
  template:
    metadata:
      labels:
        app: fast-grow-app
    spec:
      containers:
      - name: fast-grow-app
        image: rudrakshladdha/fast-grow-app:latest
        ports:
        - containerPort: 5000
```

**app-service.yaml:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: fast-grow-app
spec:
  selector:
    app: fast-grow-app
  ports:
    - protocol: TCP
      port: 5000
      targetPort: 5000
  type: LoadBalancer
```

**data-deployment.yaml:**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: fast-grow-data
spec:
  replicas: 2
  selector:
    matchLabels:
      app: fast-grow-data
  template:
    metadata:
      labels:
        app: fast-grow-data
    spec:
      containers:
      - name: fast-grow-data
        image: rudrakshladdha/fast-grow-data:latest
        ports:
        - containerPort: 5001
```

**data-service.yaml:**

```yaml
apiVersion: v1
kind: Service
metadata:
  name: fast-grow-data
spec:
  selector:
    app: fast-grow-data
  ports:
    - protocol: TCP
      port: 5001
      targetPort: 5001
  type: ClusterIP
```

##### Applying the Manifests

I applied the manifests using the following commands:

```bash
kubectl apply -f app-deployment.yaml
kubectl apply -f app-service.yaml
kubectl apply -f data-deployment.yaml
kubectl apply -f data-service.yaml
```

##### Troubleshooting Deployment Issues

Upon deployment, I encountered issues where the pods were stuck in the `ContainerCreating` state. This was due to the images not being available in my Docker Hub repository.

To resolve this, I ensured that the images were correctly built and pushed to Docker Hub:

```bash
docker build -t rudrakshladdha/fast-grow-app:latest .
docker push rudrakshladdha/fast-grow-app:latest

docker build -t rudrakshladdha/fast-grow-data:latest .
docker push rudrakshladdha/fast-grow-data:latest
```

After updating the images, the pods successfully transitioned to the `Running` state.

---

#### Accessing the Services

##### Minikube Setup

I used Minikube to create a local Kubernetes cluster. To expose the services, I used Minikube’s service command:

```bash
minikube service fast-grow-app
```

To handle the LoadBalancer service locally, I also started the Minikube tunnel:

```bash
minikube tunnel
```

##### Verifying the Services

I verified the services by accessing the Minikube IP and the node port for the `fast-grow-app` service. This confirmed that the application was successfully running in the Kubernetes cluster.

---

#### Conclusion

This journey of containerizing and deploying a Flask application using Docker and Kubernetes was a valuable learning experience. I encountered and resolved various issues, from missing files in the Docker build process to managing Kubernetes deployments and services. The key takeaways include the importance of properly managing dependencies, understanding Docker and Kubernetes configurations, and iterative troubleshooting.

By following a methodical approach, I was able to get my Flask application up and running in a containerized environment, demonstrating the power and flexibility of modern DevOps practices.

For more details about the project, visit [Fast Grow on GitHub](https://github.com/Rudraksh2003/Fast-Grow).