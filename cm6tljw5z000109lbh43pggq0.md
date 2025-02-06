---
title: "Linux Command Cheat Sheet 🐧"
datePublished: Thu Feb 06 2025 17:13:43 GMT+0000 (Coordinated Universal Time)
cuid: cm6tljw5z000109lbh43pggq0
slug: linux-command-cheat-sheet
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/4Mw7nkQDByk/upload/5d868c4688fd45ef1e9892da48863b75.jpeg
tags: twitter, linux, technology, devops, commands, linux-for-beginners

---

Here's a Linux Command Cheat Sheet for quick reference! 🚀

---

## 🔹 **File & Directory Management**

| Command | Description |
| --- | --- |
| `ls` | List files in a directory |
| `ls -l` | List files in long format |
| `ls -a` | Show hidden files |
| `pwd` | Print current working directory |
| `cd <dir>` | Change directory |
| `cd ..` | Move up one directory |
| `mkdir <dir>` | Create a new directory |
| `rmdir <dir>` | Remove an empty directory |
| `rm <file>` | Delete a file |
| `rm -r <dir>` | Remove a directory and its contents |
| `cp <source> <dest>` | Copy a file |
| `mv <source> <dest>` | Move or rename a file |
| `touch <file>` | Create a new empty file |
| `find <dir> -name <filename>` | Search for a file |
| `tree` | Display directory structure |

---

## 🔹 **File Permissions**

| Command | Description |
| --- | --- |
| `chmod 755 <file>` | Change file permissions (owner: rwx, group: r-x, others: r-x) |
| `chown user:group <file>` | Change file owner and group |
| `ls -l` | View file permissions |
| `umask 022` | Default permissions for new files |

---

## 🔹 **File Content & Editing**

| Command | Description |
| --- | --- |
| `cat <file>` | View file contents |
| `tac <file>` | View file in reverse |
| `less <file>` | View file one page at a time |
| `head -n <num> <file>` | View first N lines of a file |
| `tail -n <num> <file>` | View last N lines of a file |
| `nano <file>` | Open file in Nano editor |
| `vim <file>` | Open file in Vim editor |
| `echo "text" > <file>` | Write text to a file (overwrite) |
| `echo "text" >> <file>` | Append text to a file |
| `grep "text" <file>` | Search for text in a file |
| `wc -l <file>` | Count number of lines in a file |
| `diff <file1> <file2>` | Compare two files |
| `sort <file>` | Sort lines in a file |

---

## 🔹 **User Management**

| Command | Description |
| --- | --- |
| `whoami` | Show current user |
| `who` | Show logged-in users |
| `id` | Show user ID and group ID |
| `adduser <user>` | Add a new user |
| `passwd <user>` | Change user password |
| `deluser <user>` | Delete a user |
| `groupadd <group>` | Create a new group |
| `usermod -aG <group> <user>` | Add user to a group |

---

## 🔹 **Process Management**

| Command | Description |
| --- | --- |
| `ps aux` | Show all running processes |
| `top` | Display active processes in real-time |
| `htop` | Interactive process viewer (if installed) |
| `kill <PID>` | Kill a process by PID |
| `killall <process>` | Kill all processes by name |
| `pkill <process>` | Kill process by name |
| `nohup <command> &` | Run command in background (ignore hangups) |

---

## 🔹 **Networking**

| Command | Description |
| --- | --- |
| `ping <host>` | Check connectivity to a host |
| `curl <URL>` | Fetch content from a URL |
| `wget <URL>` | Download a file from a URL |
| `ifconfig` | Show network interfaces (deprecated) |
| `ip addr show` | Show network interfaces (modern alternative) |
| `netstat -tulnp` | List open ports |
| `ss -tulnp` | Alternative to `netstat` for open ports |
| `scp <file> user@host:/path` | Copy files over SSH |
| `rsync -avz <source> <dest>` | Sync files/directories |
| `hostname -I` | Show local IP address |

---

## 🔹 **Disk & Storage**

| Command | Description |
| --- | --- |
| `df -h` | Show disk space usage |
| `du -sh <dir>` | Show size of a directory |
| `lsblk` | Show block devices (partitions) |
| `mount /dev/sdX /mnt` | Mount a disk partition |
| `umount /mnt` | Unmount a partition |
| `fdisk -l` | Show partition table |

---

## 🔹 **System Monitoring**

| Command | Description |
| --- | --- |
| `uptime` | Show system uptime |
| `free -h` | Show memory usage |
| `vmstat` | Show system performance |
| `iostat` | Show CPU & disk stats |
| `dmesg` | Show boot log messages |

---

## 🔹 **Package Management**

🔹 **Debian/Ubuntu (**`apt`)

| Command | Description |
| --- | --- |
| `sudo apt update` | Update package list |
| `sudo apt upgrade` | Upgrade installed packages |
| `sudo apt install <package>` | Install a package |
| `sudo apt remove <package>` | Remove a package |
| `dpkg -l` | List installed packages |

🔹 **Red Hat/CentOS (**`yum` or `dnf`)

| Command | Description |
| --- | --- |
| `sudo yum install <package>` | Install a package |
| `sudo yum remove <package>` | Remove a package |
| `sudo yum update` | Update system |

🔹 **Arch (**`pacman`)

| Command | Description |
| --- | --- |
| `sudo pacman -S <package>` | Install a package |
| `sudo pacman -R <package>` | Remove a package |

---

## 🔹 **Compression & Archiving**

| Command | Description |
| --- | --- |
| `tar -cvf archive.tar <files>` | Create a tar archive |
| `tar -xvf archive.tar` | Extract a tar archive |
| `tar -czvf archive.tar.gz <files>` | Create a gzip-compressed tar archive |
| `tar -xzvf archive.tar.gz` | Extract a gzip-compressed tar archive |
| `zip` [`archive.zip`](http://archive.zip) `<files>` | Create a zip archive |
| `unzip` [`archive.zip`](http://archive.zip) | Extract a zip file |

---

## 🔹 **Permissions & Ownership**

| Command | Description |
| --- | --- |
| `chmod 755 <file>` | Change file permissions (rwx for owner, rx for others) |
| `chmod +x <file>` | Make file executable |
| `chown user:group <file>` | Change file owner/group |
| `sudo` | Execute command as root |

---

## 🔹 **Shortcuts & Misc**

| Command | Description |
| --- | --- |
| `clear` | Clear terminal screen |
| `history` | Show command history |
| `alias ll='ls -la'` | Create a shortcut for a command |
| `unalias ll` | Remove an alias |
| `!!` | Repeat the last command |
| `CTRL + C` | Stop a running process |
| `CTRL + Z` | Suspend a process |
| `bg` | Resume a suspended process in background |
| `fg` | Resume a process in foreground |

---

[📥 Grab your free PDF here of this cheat sheet](https://github.com/Rudraksh2003/Learner-Stack/blob/3eab4fb84b91161d3f4d6705a6caa7b7c0310488/Linux%20Command%20Cheat%20Sheet%20%F0%9F%90%A7.pdf)

[Visit my website](https://portfolio-e2f6.vercel.app/)