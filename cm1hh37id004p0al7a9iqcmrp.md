---
title: "Your Guide to Linux Interview Success: 60 Must-Know Questions"
datePublished: Wed Sep 25 2024 06:17:00 GMT+0000 (Coordinated Universal Time)
cuid: cm1hh37id004p0al7a9iqcmrp
slug: your-guide-to-linux-interview-success-60-must-know-questions
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/xbEVM6oJ1Fs/upload/fc8eba6db3b4bf8f2cb9a737e120c0ea.jpeg
tags: interview, linux, questions, jobs, interview-questions, linux-for-beginners, linux-basics, linux-commands

---

### System Administration & Commands

1. **What is the purpose of the** `/etc/fstab` file, and how does it work?
    
    * **Answer**: The `/etc/fstab` file is a configuration file that contains information about disk drives and partitions and how they should be mounted by the system. Each line specifies a device, its mount point, filesystem type, and mount options. The system uses this file during boot to mount filesystems automatically.
        
2. **How do you check disk usage by file and directory size on a Linux system?**
    
    * **Answer**: You can use the `du` (disk usage) command to check the size of files and directories:
        
        ```bash
        du -sh *
        ```
        
        This command provides a summary (`-s`) and human-readable (`-h`) sizes for each item in the current directory.
        
3. **What is the difference between a hard link and a soft link (symlink) in Linux?**
    
    * **Answer**: A hard link is a direct reference to the inode of a file on disk, allowing multiple filenames to reference the same file. If the original file is deleted, the hard link remains functional. A soft link (symlink) is a reference to another filename, which can point to files on different file systems. If the original file is deleted, the symlink becomes broken.
        
4. **Explain how the** `chmod`, `chown`, and `chgrp` commands work.
    
    * **Answer**:
        
        * `chmod`: Changes the file permissions. For example, `chmod 755 filename` grants read, write, and execute permissions to the owner and read and execute permissions to others.
            
        * `chown`: Changes the file owner. For example, `chown user:group filename` changes the file's owner to `user` and group to `group`.
            
        * `chgrp`: Changes the group ownership of a file. For example, `chgrp groupname filename` changes the group of the file to `groupname`.
            
5. **How do you view running processes on a Linux system? What are the differences between** `ps`, `top`, and `htop`?
    
    * **Answer**:
        
        * `ps`: Displays a snapshot of current processes. For example, `ps aux` shows all processes.
            
        * `top`: Provides a dynamic real-time view of running processes, updating periodically.
            
        * `htop`: An enhanced version of `top` that allows for easy navigation and process management, including killing processes directly.
            
6. **How can you schedule tasks in Linux using** `cron` and `at`?
    
    * **Answer**:
        
        * `cron`: Used for recurring tasks. You can edit the cron jobs using `crontab -e`, where each line represents a scheduled task with time and date fields.
            
        * `at`: Used for one-time tasks. You can schedule a task using `echo "command" | at 10:00`, which will execute the command at 10:00 AM.
            
7. **What is the purpose of the** `/proc` directory in Linux?
    
    * **Answer**: The `/proc` directory is a virtual filesystem that provides process and system information in real-time. It contains files that represent kernel and process information, such as CPU statistics, memory usage, and process details.
        
8. **How do you check system logs in Linux, and where are they stored by default?**
    
    * **Answer**: System logs are typically stored in the `/var/log` directory. You can view logs using commands like `cat`, `less`, or `tail`. For example, to view the system log:
        
        ```bash
        less /var/log/syslog
        ```
        
9. **Explain the purpose and usage of the** `sudo` command.
    
    * **Answer**: The `sudo` command allows a permitted user to execute a command as the superuser or another user, as specified by the security policy. It is used to perform administrative tasks without logging in as the root user. For example:
        
        ```bash
        sudo apt update
        ```
        
10. **What is the difference between** `kill`, `killall`, and `pkill`?
    
    * **Answer**:
        
        * `kill`: Sends a signal to a process by PID. For example, `kill 1234` sends the default SIGTERM to process 1234.
            
        * `killall`: Sends a signal to all processes by name. For example, `killall firefox` terminates all Firefox processes.
            
        * `pkill`: Sends a signal to processes based on name and other attributes. For example, `pkill -u user` sends a signal to all processes owned by the specified user.
            
11. **How do you create and manage user accounts in Linux, and what files are involved in user management?**
    
    * **Answer**: You can create a user account using the `useradd` command:
        
        ```bash
        sudo useradd username
        ```
        
        To set the password, use:
        
        ```bash
        sudo passwd username
        ```
        
        User information is stored in `/etc/passwd`, and passwords are hashed in `/etc/shadow`.
        
12. **How do you change the default shell for a user in Linux?**
    
    * **Answer**: You can change the default shell using the `chsh` command:
        
        ```bash
        chsh -s /bin/bash username
        ```
        
13. **What is a runlevel, and how do you manage runlevels in Linux (now replaced by systemd targets)?**
    
    * **Answer**: A runlevel is a predefined state of the machine that determines which services are running. Common runlevels include 0 (halt), 1 (single-user), 3 (multi-user), and 5 (multi-user with GUI). You can manage runlevels using the `init` command or `systemctl` with systemd.
        
14. **What are inodes, and how are they used in Linux file systems?**
    
    * **Answer**: An inode is a data structure on a filesystem that stores information about a file or directory, such as its size, ownership, permissions, and data block locations. Each file has a unique inode number.
        
15. **How do you extend a partition and resize a file system in Linux without losing data?**
    
    * **Answer**: You can use `resize2fs` for ext2/3/4 filesystems and `lvextend` for LVM (Logical Volume Management). For example:
        
        ```bash
        sudo lvextend -L +10G /dev/mapper/vg0-lv0
        sudo resize2fs /dev/mapper/vg0-lv0
        ```
        
16. **How do you mount and unmount file systems in Linux?**
    
    * **Answer**: To mount a filesystem, use:
        
        ```bash
        sudo mount /dev/sdb1 /mnt
        ```
        
        To unmount, use:
        
        ```bash
        sudo umount /mnt
        ```
        
17. **What is the difference between swap memory and physical memory (RAM) in Linux?**
    
    * **Answer**: Swap memory is disk space used to extend physical memory (RAM) when the RAM is full. It allows the system to run larger applications but is slower than RAM. Linux uses a swap space to manage memory efficiently.
        
18. **How do you monitor disk I/O activity in real-time?**
    
    * **Answer**: You can use the `iostat` command from the `sysstat` package to monitor disk I/O statistics:
        
        ```bash
        iostat -x 1
        ```
        
        This command provides extended I/O statistics every second.
        
19. **Explain the significance of** `/dev/null` and how you use it in Linux commands.
    
    * **Answer**: `/dev/null` is a special file that discards all data written to it, acting as a black hole. It is used to suppress output from commands. For example:
        
        ```bash
        command > /dev/null 2>&1
        ```
        
        This command redirects both stdout and stderr to `/dev/null`.
        
20. **What is the purpose of the** `strace` command, and how do you use it for debugging?
    
    * **Answer**: The `strace` command is used to trace system calls made by a program. It helps in debugging by showing how a program interacts with the kernel. For example:
        
        ```bash
        strace -o output.txt ./my_program
        ```
        
        This command runs `my_program` and logs all system calls to `output.txt`.
        

### Networking & Security

21. **Explain how the** `iptables` firewall works in Linux and how you would create basic rules.
    
    * **Answer**: `iptables` is a user-space utility for configuring the Linux kernel firewall. It works by filtering packets based on predefined rules. For example, to allow HTTP traffic:
        
        ```bash
        sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
        ```
        
        To save the rules, use:
        
        ```bash
        sudo iptables-save > /etc/iptables/rules.v4
        ```
        
22. **What is the difference between TCP and UDP, and how can you list the services using specific ports?**
    
    * **Answer**: TCP (Transmission Control Protocol) is connection-oriented and ensures reliable data delivery, while UDP (User Datagram Protocol) is connectionless and does not guarantee delivery. To list services using specific ports, you can use:
        
        ```bash
        netstat -tuln
        ```
        
23. **How do you check the active network connections and listening services on a Linux system?**
    
    * **Answer**: You can use the `ss` command to check active network connections and listening services:
        
        ```bash
        ss -tuln
        ```
        
24. **How do you troubleshoot DNS issues in Linux, and what commands do you use?**
    
    * **Answer**: You can troubleshoot DNS issues using commands like `dig`, `nslookup`, and `host`. For example:
        
        ```bash
        dig example.com
        ```
        
        This command queries DNS servers for the IP address of [`example.com`](http://example.com).
        
25. **How do you configure static IP addresses and DNS settings on a Linux system?**
    
    * **Answer**: For a static IP configuration, you can edit the `/etc/network/interfaces` file (on Debian-based systems) or use NetworkManager. For example, in `/etc/network/interfaces`:
        
        ```bash
        iface eth0 inet static
        address 192.168.1.10
        netmask 255.255.255.0
        gateway 192.168.1.1
        dns-nameservers 8.8.8.8
        ```
        
26. **What is** `SELinux`, and how does it work to enhance security in Linux?
    
    * **Answer**: SELinux (Security-Enhanced Linux) is a security module that provides mandatory access control (MAC). It restricts users and programs to the minimum permissions necessary, protecting the system from misconfigured services and vulnerabilities. You can check the status with:
        
        ```bash
        sestatus
        ```
        
27. **Explain how to set up SSH key-based authentication for a Linux server.**
    
    * **Answer**: To set up SSH key-based authentication:
        
        1. Generate an SSH key pair:
            
            ```bash
            ssh-keygen
            ```
            
        2. Copy the public key to the server:
            
            ```bash
            ssh-copy-id user@remote_host
            ```
            
        3. Log in to the server without a password.
            
28. **How do you use** `scp` and `rsync` for secure file transfer between Linux machines?
    
    * **Answer**:
        
        * `scp`: Securely copies files between hosts. For example:
            
            ```bash
            scp file.txt user@remote_host:/path/to/destination
            ```
            
        * `rsync`: Synchronizes files/directories between local and remote systems. For example:
            
            ```bash
            rsync -avz /local/dir user@remote_host:/remote/dir
            ```
            
29. **What is the purpose of the** `/etc/hosts` file, and how is it used?
    
    * **Answer**: The `/etc/hosts` file maps hostnames to IP addresses. It allows for local name resolution before querying DNS. For example:
        
        ```plaintext
        127.0.0.1 localhost
        192.168.1.10 myserver.local
        ```
        
30. **How can you restrict user access to a Linux system using** `iptables` or `/etc/hosts.deny`?
    
    * **Answer**: To restrict user access with `iptables`, you can drop connections from specific IP addresses:
        
        ```bash
        sudo iptables -A INPUT -s 192.168.1.100 -j DROP
        ```
        
        Using `/etc/hosts.deny`, you can deny access to all services:
        
        ```plaintext
        ALL: 192.168.1.100
        ```
        
31. **How do you use the** `tcpdump` command to capture network traffic on a specific interface?
    
    * **Answer**: The `tcpdump` command captures and analyzes network traffic. To capture packets on a specific interface:
        
        ```bash
        sudo tcpdump -i eth0
        ```
        
32. **What is the purpose of the** `ufw` firewall in Linux, and how do you manage firewall rules with it?
    
    * **Answer**: `ufw` (Uncomplicated Firewall) is a user-friendly interface for managing firewall rules. To enable it:
        
        ```bash
        sudo ufw enable
        ```
        
        To allow SSH connections:
        
        ```bash
        sudo ufw allow ssh
        ```
        
33. **How do you create a secure tunnel between two Linux systems using** `ssh`?
    
    * **Answer**: You can create a secure tunnel using SSH port forwarding. For example, to forward local port 8080 to remote port 80:
        
        ```bash
        ssh -L 8080:localhost:80 user@remote_host
        ```
        
34. **How would you troubleshoot slow network performance on a Linux system?**
    
    * **Answer**: Troubleshooting slow network performance involves checking network configuration, using tools like `ping`, `traceroute`, `iperf`, and monitoring bandwidth usage with `iftop` or `nload`.
        
35. **Explain the difference between symmetric and asymmetric encryption and how you can implement them on a Linux system.**
    
    * **Answer**: Symmetric encryption uses a single key for both encryption and decryption, while asymmetric encryption uses a pair of keys (public and private). You can implement symmetric encryption using tools like `openssl`:
        
        ```bash
        openssl enc -aes-256-cbc -salt -in file.txt -out file.enc
        ```
        
        For asymmetric encryption:
        
        ```bash
        openssl genpkey -algorithm RSA -out private.pem
        openssl rsa -pubout -in private.pem -out public.pem
        ```
        
36. **What are the common security best practices for securing a Linux server?**
    
    * **Answer**: Common best practices include:
        
        * Keep the system and applications updated.
            
        * Use strong passwords and change them regularly.
            
        * Disable unused services and ports.
            
        * Implement firewalls and configure `iptables` or `ufw`.
            
        * Use SSH key-based authentication and disable root login.
            
        * Regularly back up data and logs.
            

### Processes & Performance

37. **How do you change the priority of a process in Linux using** `nice` and `renice`?
    
    * **Answer**: You can set the priority of a new process using `nice`:
        
        ```bash
        nice -n 10 command
        ```
        
        To change the priority of an existing process:
        
        ```bash
        renice 10 -p 1234
        ```
        
38. **What is a daemon process in Linux, and how does it differ from regular processes?**
    
    * **Answer**: A daemon process is a background service that runs independently of user control, typically starting at boot. It often listens for requests and performs tasks, whereas regular processes interact directly with users.
        
39. **How do you find out which process is using the most CPU or memory in real-time?**
    
    * **Answer**: You can use the `top` or `htop` commands to view real-time resource usage. The `ps` command can also be used to filter processes by resource usage:
        
        ```bash
        ps aux --sort=-%mem | head -n 10  # Top 10 memory-consuming processes
        ```
        
40. **How do you limit the CPU or memory usage of a process in Linux?**
    
    * **Answer**: You can use `cpulimit` to limit CPU usage:
        
        ```bash
        cpulimit -l 50 -p 1234  # Limit process 1234 to 50% CPU
        ```
        
        To limit memory usage, you can use `ulimit`:
        
        ```bash
        ulimit -v 500000  # Limit virtual memory to 500 MB
        ```
        
41. **Explain how memory is managed in Linux, including concepts like virtual memory and memory swapping.**
    
    * **Answer**: Linux uses virtual memory to allow processes to use more memory than physically available. The kernel manages this by swapping out inactive pages to swap space on disk, freeing up RAM for active processes. The `vmstat` command can provide insights into memory usage and swapping.
        
42. **What is a zombie process, and how can you identify and deal with it in Linux?**
    
    * **Answer**: A zombie process is a process that has completed execution but still has an entry in the process table, typically because its parent hasn't read its exit status. You can identify zombie processes using `ps aux | grep Z`. They are usually cleared when the parent process reads their status.
        
43. **How does the** `systemd` init system work in Linux, and how do you manage services with it?
    
    * **Answer**: `systemd` is an init system that initializes user space and manages system processes. You can manage services using the `systemctl` command:
        
        ```bash
        sudo systemctl start service_name
        sudo systemctl enable service_name
        sudo systemctl status service_name
        ```
        
44. **How do you use the** `vmstat` command to monitor system performance?
    
    * **Answer**: The `vmstat` command provides a summary of system performance, including memory, swap, I/O, and CPU usage. For example:
        
        ```bash
        vmstat 1 5  # Provides stats every second for 5 seconds
        ```
        
45. **How do you diagnose and troubleshoot a high load average on a Linux system?**
    
    * **Answer**: To diagnose high load average, check the output of `top` or `htop` to identify resource-heavy processes. Analyze CPU and memory usage, disk I/O, and check for any processes stuck in uninterruptible sleep (`D` state). Use `iostat` and `vmstat` for deeper insights.
        

46

. **What are the main differences between** `systemd` and `init.d`? - **Answer**: `systemd` is a modern init system that manages services and dependencies efficiently, supports parallel service startup, and provides better logging with `journald`. `init.d` uses a traditional method with scripts for service management and starts services sequentially.

### Disk Management

47. **How do you check disk usage and available space in Linux?**
    
    * **Answer**: You can use the `df` command to check disk usage:
        
        ```bash
        df -h  # Shows human-readable disk space usage
        ```
        
        For more detailed information about individual directories, use `du`:
        
        ```bash
        du -sh /path/to/directory
        ```
        
48. **Explain how to create and manage disk partitions in Linux.**
    
    * **Answer**: You can create and manage disk partitions using tools like `fdisk`, `parted`, or `gparted`. For example, to create a new partition with `fdisk`:
        
        ```bash
        sudo fdisk /dev/sda
        ```
        
        Follow the prompts to create a new partition.
        
49. **How do you format a disk with a specific filesystem type?**
    
    * **Answer**: To format a disk with a specific filesystem type (e.g., ext4):
        
        ```bash
        sudo mkfs.ext4 /dev/sdb1  # Replace /dev/sdb1 with the actual device
        ```
        
50. **What is the purpose of the** `fstab` file, and how is it used?
    
    * **Answer**: The `/etc/fstab` file contains information about filesystems and their mount points. It is used to automatically mount filesystems at boot. An entry might look like:
        
        ```plaintext
        /dev/sdb1  /mnt/data  ext4  defaults  0  2
        ```
        
51. **How do you check and repair a corrupted filesystem in Linux?**
    
    * **Answer**: You can check and repair a corrupted filesystem using `fsck`:
        
        ```bash
        sudo fsck /dev/sdb1  # Replace with the actual device
        ```
        
52. **What is LVM, and how does it benefit disk management in Linux?**
    
    * **Answer**: LVM (Logical Volume Manager) allows for flexible disk management by creating logical volumes that can be resized and managed independently of the underlying physical disks. It facilitates easier partition resizing and better storage utilization.
        
53. **How do you create a disk image of a partition in Linux?**
    
    * **Answer**: You can create a disk image using the `dd` command:
        
        ```bash
        sudo dd if=/dev/sda1 of=/path/to/image.img bs=4M
        ```
        
54. **What is the purpose of the** `swap` space in Linux, and how do you configure it?
    
    * **Answer**: Swap space is used when the physical RAM is full, allowing the system to continue functioning by temporarily moving inactive pages to disk. To configure swap, you can create a swap file:
        
        ```bash
        sudo fallocate -l 1G /swapfile
        sudo chmod 600 /swapfile
        sudo mkswap /swapfile
        sudo swapon /swapfile
        ```
        

### User & Group Management

55. **How do you create a new user and set a password in Linux?**
    
    * **Answer**: You can create a new user and set a password using the following commands:
        
        ```bash
        sudo adduser newuser
        sudo passwd newuser
        ```
        
56. **What are the different user types in Linux, and how do they differ?**
    
    * **Answer**: The main user types are:
        
        * **Root**: The superuser with full access to the system.
            
        * **Regular users**: Have limited permissions, typically their own home directories.
            
        * **Service accounts**: Used by system services, often with restricted access.
            
57. **How do you manage user groups in Linux?**
    
    * **Answer**: You can manage user groups using the following commands:
        
        * To create a group:
            
            ```bash
            sudo groupadd groupname
            ```
            
        * To add a user to a group:
            
            ```bash
            sudo usermod -aG groupname username
            ```
            
58. **How do you change file permissions and ownership in Linux?**
    
    * **Answer**: Use the `chmod` command to change permissions:
        
        ```bash
        chmod 755 file.txt
        ```
        
        Use the `chown` command to change ownership:
        
        ```bash
        sudo chown user:group file.txt
        ```
        
59. **What is the purpose of the** `/etc/passwd` and `/etc/shadow` files?
    
    * **Answer**: The `/etc/passwd` file contains user account information, including username, user ID, group ID, and home directory. The `/etc/shadow` file stores encrypted user passwords and password expiration information, providing an additional layer of security.
        
60. **How do you view and manage user login attempts in Linux?**
    
    * **Answer**: You can view login attempts in the `/var/log/auth.log` file (on Debian-based systems) or the `/var/log/secure` file (on Red Hat-based systems). Use commands like `last` to view user login history:
        
        ```bash
        last
        ```