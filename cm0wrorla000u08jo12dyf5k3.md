---
title: "🚀 Comprehensive Guide to GitHub Actions: From Basics to Advanced"
seoTitle: "🚀 Comprehensive Guide to GitHub Actions: From Basics to Advanced"
datePublished: Tue Sep 10 2024 18:30:32 GMT+0000 (Coordinated Universal Time)
cuid: cm0wrorla000u08jo12dyf5k3
slug: comprehensive-guide-to-github-actions-from-basics-to-advanced
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/wX2L8L-fGeA/upload/7cfa756169b26722a349c75e44afc197.jpeg
tags: cloud, aws, github, continuous-integration, continuous-deployment, computer-science, devops, containers, jenkins, 2articles1week, github-actions-1

---

## Introduction 🌟

GitHub Actions is a powerful tool integrated into GitHub that automates your software development workflows. It allows you to build, test, and deploy code directly within GitHub. This guide covers everything from basic to advanced concepts, including practical implementations, a comparison with Jenkins, and how to use GitHub Actions with AWS Cloud.

## Understanding GitHub Actions 🤔

**GitHub Actions** is a feature of GitHub for automating workflows through YAML configuration files. These files are located in the `.github/workflows` directory of your repository and define how your workflows are triggered and executed.

![Actions in Github. It has been a while now since Github… | by Dhruv Patel |  Treebo Tech Blog](https://miro.medium.com/v2/resize:fit:1000/1*ookyGlsTh-UT0YBUwdTLSA.png align="left")

### Key Components 🔑

* **Actions:** Reusable tasks that can be used in workflows. Use pre-built actions from the GitHub Marketplace or create your own.
    
* **Workflows:** Define automated processes using YAML files. A workflow contains one or more jobs.
    
* **Jobs:** A set of steps executed on the same runner. Jobs can run sequentially or in parallel.
    
* **Steps:** Individual commands or actions that are executed as part of a job.
    
* **Runners:** Servers that execute your workflows. GitHub provides hosted runners, and you can also use self-hosted runners.
    

## Basic Concepts 🛠️

### Creating a Workflow

To create a basic workflow:

1. **Create a Directory:** Create a directory in your repository named `.github/workflows`.
    
2. **Create a YAML File:** Create a file in this directory, e.g., `ci.yml`.
    
3. **Define a Workflow:**
    
    ```yaml
    name: CI
    
    on: [push]
    
    jobs:
      build:
        runs-on: ubuntu-latest
    
        steps:
          - name: Checkout code
            uses: actions/checkout@v2
    
          - name: Set up Node.js
            uses: actions/setup-node@v2
            with:
              node-version: '14'
    
          - name: Install dependencies
            run: npm install
    
          - name: Run tests
            run: npm test
    ```
    
    **Explanation:**
    
    * `name:` The workflow’s name.
        
    * `on:` Defines the event that triggers the workflow (e.g., `push`).
        
    * `jobs:` Contains jobs to be executed.
        
    * `runs-on:` Specifies the runner environment (e.g., `ubuntu-latest`).
        
    * `steps:` Lists the steps for the job.
        

### Running the Workflow 🚀

Once you push the YAML file to your repository, GitHub Actions will automatically run the workflow based on the specified trigger. Monitor the progress and results on the “Actions” tab of your GitHub repository.

## Advanced Concepts 🌐

### Workflow Triggers ⏰

Customize workflow triggers:

* `push` and `pull_request:` Trigger workflows on code changes.
    
* `schedule:` Trigger workflows on a schedule using cron syntax.
    
* `workflow_dispatch:` Manually trigger workflows from the GitHub interface.
    

Example of a scheduled workflow:

```yaml
name: Scheduled Workflow

on:
  schedule:
    - cron: '0 0 * * *'  # Every day at midnight

jobs:
  example:
    runs-on: ubuntu-latest
    steps:
      - name: Echo Hello World
        run: echo "Hello, world!"
```

### Matrix Builds 🔄

Run tests across multiple environments:

```yaml
name: CI

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12, 14, 16]

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: ${{ matrix.node-version }}

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test
```

### Secrets and Environments 🔐

Store sensitive information in GitHub Secrets and use them in your workflows:

1. **Add Secrets:** Go to repository settings, then "Secrets" and add new secrets.
    
2. **Use Secrets:** Reference secrets in your workflow YAML file:
    
    ```yaml
    - name: Deploy
      env:
        API_KEY: ${{ secrets.API_KEY }}
      run: echo "Deploying with API key $API_KEY"
    ```
    

### Breakdown of GitHub Actions YAML Configuration 📝

Here’s a detailed breakdown of the YAML configuration:

```yaml
name: CI

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'

      - name: Install dependencies
        run: npm install

      - name: Run tests
        run: npm test

      - name: Deploy
        env:
          API_KEY: ${{ secrets.API_KEY }}
        run: |
          echo "Deploying with API key $API_KEY"
          # Add deployment script here
```

* `name:` The name of the workflow.
    
* `on:` Specifies the event that triggers the workflow (`push` to `main` branch).
    
* `jobs:` Defines one or more jobs. Each job runs on a specified runner environment.
    
* `steps:` Lists the steps for the job.
    
* `uses:` Refers to pre-built actions.
    
* `run:` Executes shell commands.
    
* `env:` Sets environment variables, including secrets accessed via `${{` [`secrets.NAME`](http://secrets.NAME) `}}`.
    

## Practical Implementation 🔧

**Use Case: Automated Deployment**

1. **Create a Workflow File:**
    
    ```yaml
    name: Deploy to Production
    
    on:
      push:
        branches:
          - main
    
    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - name: Checkout code
            uses: actions/checkout@v2
    
          - name: Set up SSH
            uses: webfactory/ssh-agent@v0.5.3
            with:
              ssh-private-key: ${{ secrets.SSH_PRIVATE_KEY }}
    
          - name: Deploy
            run: ssh user@server 'deploy-script.sh'
    ```
    
2. **Define Secrets:** Add `SSH_PRIVATE_KEY` to your repository secrets.
    
3. **Push Changes:** Pushing to the `main` branch will trigger the deployment workflow.
    

# Using GitHub Actions with AWS Cloud ☁️

GitHub Actions can integrate seamlessly with AWS Cloud for CI/CD pipelines. Here’s how you can set it up:

![Securely integrate AWS Credentials with GitHub Actions using OpenID Connect  – My Devops Journal](https://skundunotes.com/wp-content/uploads/2023/02/71-image-1-1.png?w=1200 align="left")

## Setup AWS Credentials

1. **Create AWS Access Keys:** Go to the AWS Management Console and create access keys in the IAM section.
    
2. **Add Secrets to GitHub:** Go to your repository’s settings, then "Secrets" and add the following secrets:
    
    * `AWS_ACCESS_KEY_ID`
        
    * `AWS_SECRET_ACCESS_KEY`
        

## Example Workflow for Deploying to AWS

To integrate AWS EC2 with your GitHub Actions pipeline, you can set up steps to connect to your EC2 instance via SSH, and then deploy your Docker containers to it. This involves:

1. **Adding an EC2 SSH Key as a GitHub Secret**: You’ll need to store the private key used for SSH access to your EC2 instance as a GitHub secret.
    
2. **Adding SSH Commands**: In your GitHub Action, use the SSH key to connect to your EC2 instance and run the necessary Docker Compose commands on the remote server.
    

Here’s how you can modify your existing GitHub Action to deploy your Docker images to an EC2 instance:

```yaml
\
name: CI Pipeline

on:
  push:
    branches:
      - main

env:
  DATA_BACKEND_IMAGE: rudraksh/fast-grow-data
  COMMENT_BACKEND_IMAGE: rudraksh/fast-grow-app
  DOCKER_USERNAME: rudraksh # Use GitHub Secrets for Docker username
  EC2_USER: ubuntu # Default user for EC2 instances using Ubuntu
  EC2_HOST: ec2-xx-xx-xx-xx.compute-1.amazonaws.com # Replace with your EC2 public IP or DNS

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v2

      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2

      - name: Install Docker Compose
        run: |
          sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
          sudo chmod +x /usr/local/bin/docker-compose
          docker-compose --version  # Verify installation

      - name: Build Comment Backend Docker Image
        run: |
          echo "Building Comment Backend Docker Image..."
          docker build -t ${{ env.COMMENT_BACKEND_IMAGE }} .
          docker tag ${{ env.COMMENT_BACKEND_IMAGE }}:latest ${{ env.COMMENT_BACKEND_IMAGE }}:latest
          echo "Comment Backend Docker Image built and tagged successfully."

      - name: Build Data Backend Docker Image
        run: |
          echo "Building Data Backend Docker Image..."
          docker build -t ${{ env.DATA_BACKEND_IMAGE }} .
          docker tag ${{ env.DATA_BACKEND_IMAGE }}:latest ${{ env.DATA_BACKEND_IMAGE }}:latest
          echo "Data Backend Docker Image built and tagged successfully."

      - name: Login to Docker Hub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ env.DOCKER_USERNAME }}" --password-stdin

      - name: Push Data Backend Docker Image
        run: |
          echo "Pushing Data Backend Docker Image..."
          docker push ${{ env.DATA_BACKEND_IMAGE }}:latest
          echo "Data Backend Docker Image pushed successfully."

      - name: Push Comment Backend Docker Image
        run: |
          echo "Pushing Comment Backend Docker Image..."
          docker push ${{ env.COMMENT_BACKEND_IMAGE }}:latest
          echo "Comment Backend Docker Image pushed successfully."

      # Deploying to EC2 instance
      - name: SSH into EC2 and deploy
        uses: appleboy/ssh-action@v0.1.3
        with:
          host: ${{ env.EC2_HOST }}
          username: ${{ env.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            echo "Pulling latest Docker images on EC2 instance..."
            docker pull ${{ env.COMMENT_BACKEND_IMAGE }}:latest
            docker pull ${{ env.DATA_BACKEND_IMAGE }}:latest
            cd /path/to/your/docker-compose # Navigate to the Docker Compose directory
            docker-compose down
            docker-compose up -d --build
            echo "Deployment on EC2 completed."

      - name: Clean up workspace
        run: |
          echo "Cleaning workspace..."
          rm -rf *
```

## **Explanation:**

### 1\. **Workflow Name**

```yaml
name: CI Pipeline
```

* `name: CI Pipeline`: This defines the name of your workflow. It will be shown in the GitHub Actions tab. In this case, it's called **"CI Pipeline"**.
    

---

### 2\. **Trigger Event**

```yaml
on:
  push:
    branches:
      - main
```

* `on`: Specifies when this workflow should run.
    
* `push`: It will run whenever you push changes to the repository.
    
* `branches: - main`: This ensures the workflow only triggers when you push changes to the `main` branch.
    

---

### 3\. **Environment Variables**

```yaml
env:
  DATA_BACKEND_IMAGE: rudrakshladdha/fast-grow-data
  COMMENT_BACKEND_IMAGE: rudrakshladdha/fast-grow-app
  DOCKER_USERNAME: rudrakshladdha # Use GitHub Secrets for Docker username
  EC2_USER: ubuntu # Default user for EC2 instances using Ubuntu
  EC2_HOST: ec2-xx-xx-xx-xx.compute-1.amazonaws.com # Replace with your EC2 public IP or DNS
```

* `env`: This section defines environment variables that will be used throughout the workflow. They include:
    
    * `DATA_BACKEND_IMAGE`: The Docker image name for your backend service.
        
    * `COMMENT_BACKEND_IMAGE`: The Docker image name for your comment service.
        
    * `DOCKER_USERNAME`: Your Docker Hub username.
        
    * `EC2_USER`: The username for SSH access to your EC2 instance (commonly `ubuntu` for Ubuntu instances).
        
    * `EC2_HOST`: The public IP or DNS of your EC2 instance. Replace with your actual EC2 details.
        

---

### 4\. **Jobs Section**

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

* `jobs`: Defines the job or series of steps the workflow will perform.
    
* `build`: The name of the job. You can name this anything (e.g., `build`, `deploy`, etc.).
    
* `runs-on: ubuntu-latest`: This specifies the virtual machine (runner) the job will run on. In this case, it uses the latest Ubuntu environment.
    

---

### 5\. **Job Steps**

Each step is a unit of execution within the job. Here's what happens in each step:

---

#### Step 1: Checkout Code

```yaml
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2
```

* `actions/checkout@v2`: This action checks out your repository's code, so the workflow can work with the files. It's like pulling the latest version of your code from GitHub into the runner.
    

---

#### Step 2: Set up Docker Buildx

```yaml
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
```

* `docker/setup-buildx-action@v2`: This step sets up Docker Buildx, which is an extended build tool for building and pushing multi-platform Docker images.
    

---

#### Step 3: Install Docker Compose

```yaml
      - name: Install Docker Compose
        run: |
          sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep tag_name | cut -d '"' -f 4)/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
          sudo chmod +x /usr/local/bin/docker-compose
          docker-compose --version  # Verify installation
```

* `run`: Runs shell commands on the runner.
    
* This step installs **Docker Compose** using the latest release from GitHub. It then gives execute permissions to the binary and verifies the installation by running `docker-compose --version`.
    

---

#### Step 4 & 5: Build Docker Images

```yaml
      - name: Build Comment Backend Docker Image
        run: |
          docker build -t ${{ env.COMMENT_BACKEND_IMAGE }} .
          docker tag ${{ env.COMMENT_BACKEND_IMAGE }}:latest ${{ env.COMMENT_BACKEND_IMAGE }}:latest

      - name: Build Data Backend Docker Image
        run: |
          docker build -t ${{ env.DATA_BACKEND_IMAGE }} .
          docker tag ${{ env.DATA_BACKEND_IMAGE }}:latest ${{ env.DATA_BACKEND_IMAGE }}:latest
```

* `docker build`: Builds the Docker image for the comment backend and data backend services, using the names defined in the environment variables.
    
* `docker tag`: Tags the images with the `latest` tag, so they can be easily identified and pushed.
    

---

#### Step 6: Docker Hub Login

```yaml
      - name: Login to Docker Hub
        run: echo "${{ secrets.DOCKER_PASSWORD }}" | docker login -u "${{ env.DOCKER_USERNAME }}" --password-stdin
```

* **Docker login**: This step logs into Docker Hub using the username and password. The password is retrieved from the GitHub Secrets (`DOCKER_PASSWORD`), while the username comes from the environment variable (`DOCKER_USERNAME`).
    

---

#### Step 7 & 8: Push Docker Images

```yaml
      - name: Push Data Backend Docker Image
        run: docker push ${{ env.DATA_BACKEND_IMAGE }}:latest

      - name: Push Comment Backend Docker Image
        run: docker push ${{ env.COMMENT_BACKEND_IMAGE }}:latest
```

* `docker push`: These steps push the built Docker images (data backend and comment backend) to Docker Hub.
    

---

#### Step 9: SSH into EC2 and Deploy

```yaml
      - name: SSH into EC2 and deploy
        uses: appleboy/ssh-action@v0.1.3
        with:
          host: ${{ env.EC2_HOST }}
          username: ${{ env.EC2_USER }}
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            docker pull ${{ env.COMMENT_BACKEND_IMAGE }}:latest
            docker pull ${{ env.DATA_BACKEND_IMAGE }}:latest
            cd /path/to/your/docker-compose
            docker-compose down
            docker-compose up -d --build
```

* `appleboy/ssh-action@v0.1.3`: This action connects to your EC2 instance via SSH.
    
* `host`, `username`, `key`: The connection details:
    
    * `host`: The public IP or DNS of your EC2 instance.
        
    * `username`: The EC2 user (usually `ubuntu`).
        
    * `key`: The SSH private key stored in GitHub Secrets (`EC2_SSH_KEY`).
        
* `script`: The commands that will run on the EC2 instance. It pulls the latest Docker images and restarts the services using Docker Compose.
    

---

#### Step 10: Clean Up Workspace

```yaml
      - name: Clean up workspace
        run: rm -rf *
```

* `rm -rf *`: This command cleans up the workspace by removing all files. It's optional and can be useful to free up space on the runner if needed.
    

---

This setup automates your CI/CD pipeline by building and deploying Dockerized applications to your AWS EC2 instance. Let me know if you need further clarification or adjustments!

# Comparison with Jenkins ⚙️

**GitHub Actions vs. Jenkins:**

![Mastering CI/CD: GitHub Actions vs Jenkins | by Safe | Medium](https://miro.medium.com/v2/resize:fit:1400/1*xkuGgYhgWO5SP1UGuBrgbA.png align="left")

* **Integration:** GitHub Actions is tightly integrated with GitHub, while Jenkins requires additional setup and plugins to integrate with GitHub.
    
* **Setup and Maintenance:** GitHub Actions offers a simpler setup with hosted runners and minimal maintenance. Jenkins requires managing Jenkins servers and plugins.
    
* **Configuration:** GitHub Actions uses YAML files for configuration, which are generally more straightforward compared to Jenkins’ XML configurations.
    
* **Extensibility:** Jenkins has a vast library of plugins for extensive customization. GitHub Actions also has a marketplace, but it is more streamlined.
    

# Conclusion 🎉

GitHub Actions provides a robust and integrated solution for automating your workflows directly within GitHub. It simplifies setup and maintenance compared to Jenkins while offering a comprehensive set of features. By understanding and leveraging Git

---

**<mark>If you find this article helpful, please consider sponsoring me for more in-depth articles and contributions. Your support helps me create even better content! 🙌</mark>**

Thank you for reading!

#GitHubActions #CI #CD #DevOps #Automation #SoftwareDevelopment #TechTips #GitHub #JenkinsComparison #YAMLConfiguration #AWS #Cloud #DevOps