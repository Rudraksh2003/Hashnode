---
title: "Using Ansible to Manage Windows Hosts"
seoTitle: "Using Ansible to Manage Windows Hosts: A Comprehensive Guide"
seoDescription: "Learn how to use Ansible to manage Windows hosts with this detailed guide. Discover step-by-step instructions, key terminologies, playbook examples, and FAQ"
datePublished: Fri Jun 28 2024 18:50:26 GMT+0000 (Coordinated Universal Time)
cuid: clxz1tbfb00000al6bmg3d6c5
slug: using-ansible-to-manage-windows-hosts
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1719600701537/c5c6c95f-8a79-479a-a1eb-4594fb326756.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1719600452274/7a666bcc-993f-4760-ab56-82ed9b0235f1.png
tags: ansible, devops, devops-articles, ansible-playbook

---

# Introduction

Managing multiple computers and servers manually can be time-consuming and error-prone. Ansible is a powerful, open-source tool that automates these tasks, making it easier to manage large-scale IT environments. Originally designed for Linux, Ansible also supports Windows systems, allowing you to manage them efficiently. This article will walk you through the process of using Ansible to manage Windows hosts, explain key concepts and provide step-by-step instructions.

## Key Concepts

**1\. Ansible:** A tool that automates IT tasks such as installing software, managing configurations, and orchestrating workflows across multiple machines.

**2\. Playbook:** A file written in YAML format that contains a list of tasks for Ansible to execute on your machines.

**3\. Inventory:** A file that lists all the machines (hosts) Ansible will manage. This can include their IP addresses, login details, and connection methods.

**4\. Module:** A pre-written set of instructions that Ansible uses to perform specific tasks on the hosts.

**5\. Task:** An individual action that Ansible performs, such as installing a program or creating a file.

**6\. Control Node:** The computer where Ansible is installed and from which you run your playbooks.

**7\. Managed Node:** The computers or servers that Ansible manages.

## Step-by-Step Process

### Step 1: Install Ansible on the Control Node

First, you need a machine to act as your control node. Typically, this is a Linux machine. Here’s how to install Ansible on it:

1. Open your terminal.
    
2. Update your package list by running:
    
    ```bash
    sudo apt update
    ```
    
3. Install Ansible by running:
    
    ```bash
    sudo apt install ansible -y
    ```
    

### Step 2: Prepare Your Windows Hosts

Your Windows computers need to have a feature called WinRM (Windows Remote Management) enabled so that Ansible can communicate with them.

1. Open PowerShell as an administrator on each Windows machine.
    
2. Run the following commands:
    
    ```powershell
    winrm quickconfig
    winrm set winrm/config/client/auth @{Basic="true"}
    winrm set winrm/config/service/auth @{Basic="true"}
    winrm set winrm/config/service @{AllowUnencrypted="true"}
    ```
    

### Step 3: Install pywinrm on the Control Node

Ansible uses a Python library called `pywinrm` to connect to Windows machines.

1. On your control node, run:
    
    ```bash
    pip install pywinrm
    ```
    

### Step 4: Create an Inventory File

An inventory file tells Ansible which machines to manage and how to connect to them.

1. Create a new file called `hosts`.
    
2. Add the following content, replacing the IP address, username, and password with your own details:
    
    ```ini
    [windows]
    192.168.1.100
    
    [windows:vars]
    ansible_user=YourUsername
    ansible_password=YourPassword
    ansible_connection=winrm
    ansible_winrm_transport=basic
    ansible_winrm_server_cert_validation=ignore
    ```
    

### Step 5: Write a Playbook

A playbook contains the tasks you want Ansible to perform on your Windows hosts. Let’s create a simple playbook that installs IIS (a web server) and creates a directory.

1. Create a file named `windows.yml`.
    
2. Add the following content:
    
    ```yaml
    ---
    - name: Manage Windows Hosts
      hosts: windows
      tasks:
        - name: Ensure IIS is installed
          win_feature:
            name: Web-Server
            state: present
    
        - name: Create a directory
          win_file:
            path: C:\Temp\example
            state: directory
    ```
    

### Step 6: Run the Playbook

Finally, run your playbook to execute the tasks on your Windows hosts.

1. In your terminal, run:
    
    ```bash
    ansible-playbook -i hosts windows.yml
    ```
    

## Example Task: Installing Software on Windows

Here’s an example of a playbook that downloads and installs Notepad++ on a Windows host:

1. Create a new playbook file named `install_notepad.yml`.
    
2. Add the following content:
    
    ```yaml
    ---
    - name: Install Notepad++
      hosts: windows
      tasks:
        - name: Download Notepad++ installer
          win_get_url:
            url: https://download.notepad-plus-plus.org/repository/7.x/7.9.5/npp.7.9.5.Installer.exe
            dest: C:\Temp\npp_installer.exe
    
        - name: Install Notepad++
          win_package:
            path: C:\Temp\npp_installer.exe
            arguments: /S
            state: present
    ```
    
3. Run the playbook:
    
    ```bash
    ansible-playbook -i hosts install_notepad.yml
    ```
    

## FAQs

**Q1: What is WinRM?** **A1:** WinRM, or Windows Remote Management, is a protocol that allows remote management of Windows machines, enabling administrators to execute commands and manage configurations remotely.

**Q2: Can I use Ansible to manage both Linux and Windows hosts?** **A2:** Yes, Ansible can manage both Linux and Windows hosts, providing a unified approach to infrastructure automation.

**Q3: Do I need to install any agents on Windows hosts to use Ansible?** **A3:** No, Ansible is agentless. It communicates with Windows hosts over WinRM without requiring any additional software on the managed nodes.

**Q4: How can I troubleshoot connectivity issues between Ansible and Windows hosts?** **A4:** Ensure WinRM is properly configured and running on the Windows hosts. Verify network connectivity and correct credentials in the inventory file.

**Q5: Is it possible to run PowerShell scripts with Ansible?** **A5:** Yes, you can use the `win_shell` module in Ansible to execute PowerShell scripts on Windows hosts.

By following these steps and examples, you can leverage Ansible to manage your Windows hosts effectively, streamlining your automation processes and improving your IT infrastructure management.