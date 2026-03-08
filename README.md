# 🖥️ Born2beroot

> A system administration project focused on virtualization, security, and Linux configuration, developed as part of the 42 School curriculum.

---

## 📖 Description

**Born2beroot** is a project from the 42 curriculum that introduces the fundamentals of **system administration** using a virtual machine.

The goal is to set up a secure Linux server following a strict set of rules, including **user management**, **password policies**, **firewall configuration**, **SSH security**, and **system monitoring**.

This repository contains **notes and documentation** created during the installation and configuration process.

---

## 🎯 Objectives

- Understand **virtualization**
- Install and configure a **Linux operating system**
- Apply **basic system security practices**
- Manage **users and groups**
- Configure **SSH access**
- Implement **password policies**
- Configure a **firewall**
- Create a **system monitoring script**

---

## 🧰 Technologies

| Tool | Purpose |
|-----|--------|
| **VirtualBox / UTM** | Virtual machine environment |
| **Debian / Rocky Linux** | Operating system |
| **SSH** | Remote secure access |
| **UFW / Firewalld** | Firewall configuration |
| **Bash** | Monitoring script |

---

## 🖥️ Virtual Machine Setup

The project requires installing a Linux system inside a virtual machine with the following general steps:

1. Create a virtual machine
2. Install the operating system
3. Configure disk partitioning
4. Install essential packages
5. Configure users and groups
6. Secure SSH access
7. Configure the firewall
8. Implement password policies

---

## 🔐 Security Configuration

The system must follow strict security rules:

- **Strong password policy**
- Password expiration
- Limited login attempts
- Secure SSH configuration
- Firewall rules enabled
- Root access restrictions

Example password policy:

| Setting | Value |
|-------|------|
| Password expiration | 30 days |
| Minimum password length | 10 characters |
| Password complexity | Uppercase, lowercase, digits |

---

## 👤 User and Group Management

Example configuration:

- Create a new user
- Assign the user to the required groups
- Restrict root login

```bash
adduser <username>
usermod -aG sudo <username>
```

## 🔥 Firewall

A firewall must be configured to allow only required services.

Example using UFW:

```
sudo ufw enable
sudo ufw allow 4242
sudo ufw status
```

## 🔑 SSH Configuration

SSH must be configured to improve security:

- Change the default port
- Disable root login
- Allow only specific users

Configuration file:

`/etc/ssh/sshd_config`

Example:

```
Port 4242
PermitRootLogin no
```

## 📊 Monitoring Script

The project requires a monitoring script that displays system information such as:

- System architecture
- CPU usage
- Memory usage
- Disk usage
- Number of logged users
- Network information
- Number of sudo commands executed

The script is typically executed periodically using cron.

Example:

`*/10 * * * * /path/to/monitoring.sh`

---

## 👤 Author

**clagarci**

42 Student

---

## 📜 License

This project was developed for educational purposes as part of the **42 School** curriculum.
