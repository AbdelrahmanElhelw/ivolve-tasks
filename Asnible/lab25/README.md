# Lab 26: Initial Ansible Configuration and Ad-Hoc Execution

## 🎯 Lab Objectives
- Install and configure Ansible Automation Platform on the control node.
- Generate a new SSH key on the control node.
- Transfer the public key into the managed nodes using the `ssh-copy-id` command.
- Create an inventory of the managed nodes.
- Perform ad-hoc commands (e.g., check disk space).

---

## 🛠️ Infrastructure Setup (Docker Managed Nodes)

### Step 1: Provision the Containers
First, spin up two Rocky Linux containers in the background to act as the target nodes.

```bash
docker run -d --name ansible-node-01 -p 2222:22 rockylinux:9 sleep infinity
docker run -d --name ansible-node-02 -p 2233:22 rockylinux:9 sleep infinity
```

### Step 2: Configure the Containers
Next, configure both containers to run the SSH daemon, create the `ansible` user with sudo privileges, and prepare them to act as managed nodes.

```bash
# Configure Node-01
docker exec -it ansible-node-01 bash -c "dnf install -y openssh-server sudo && ssh-keygen -A && useradd ansible && echo 'ansible:ansible123' | chpasswd && echo 'ansible ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers.d/ansible && mkdir -p /run/sshd && /usr/sbin/sshd"

# Configure Node-02
docker exec -it ansible-node-02 bash -c "dnf install -y openssh-server sudo && ssh-keygen -A && useradd ansible && echo 'ansible:ansible123' | chpasswd && echo 'ansible ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers.d/ansible && mkdir -p /run/sshd && /usr/sbin/sshd"
```

*Note: The internal Docker network IPs for the containers were identified as `172.17.0.2` and `172.17.0.3` using the `hostname -I` command.*

---

## 🚀 Lab Execution Steps

### Step 3: Generate New SSH Key on Control Node
Created a dedicated directory for the lab keys to keep the environment organized, and generated a 4096-bit RSA key pair.

```bash
mkdir -p ~/.ssh/ansible_lab
ssh-keygen -t rsa -b 4096 -f ~/.ssh/ansible_lab/lab_key
```

### Step 4: Transfer the Public Key to Managed Nodes
Used `ssh-copy-id` to distribute the public key to the internal IPs of the Docker containers, pointing explicitly to the custom key path.

```bash
ssh-copy-id -i ~/.ssh/ansible_lab/lab_key.pub ansible@172.17.0.2
ssh-copy-id -i ~/.ssh/ansible_lab/lab_key.pub ansible@172.17.0.3
```

### Step 5: Create Inventory File
Created an `inventory.ini` file defining the `[managed_nodes]` group, specifying the target IPs, the SSH user, and the path to the custom private key.

```ini
# inventory.ini
[managed_nodes]
172.17.0.2 ansible_user=ansible ansible_ssh_private_key_file=~/.ssh/ansible_lab/lab_key
172.17.0.3 ansible_user=ansible ansible_ssh_private_key_file=~/.ssh/ansible_lab/lab_key
```

### Step 6: Perform Ad-Hoc Commands
Verified connectivity and performed a disk space check across the managed nodes without requiring password authentication.

**1. Connectivity Test:**
```bash
ansible all -i inventory.ini -m ping
```

**2. Check Disk Space (Lab Requirement):**
```bash
ansible managed_nodes -i inventory.ini -m shell -a "df -h"
```

## ✅ Expected Output
The `df -h` command successfully returned the filesystem disk space usage for both `172.17.0.2` and `172.17.0.3` with a `CHANGED | rc=0` status, confirming successful passwordless SSH execution via Ansible.

![Ad-Hoc Command Output](./screenshots/Adhock_2.png)