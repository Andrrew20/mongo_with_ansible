# **MongoDB Deployment with Ansible**

![Ansible](https://img.shields.io/badge/ansible-2.10+-red.svg) ![MongoDB](https://img.shields.io/badge/mongodb-4.4-green.svg)

Automated deployment of MongoDB across hybrid Linux environments with intelligent gateway routing and access control.

## The purpose of playbooks

group_vars
Contains variables specific to groups of hosts from the inventory. For example, settings for different environments (prod, dev) or server roles (web, db).

inventory
A file or directory that defines the hosts and their grouping. Specifies which servers playbooks are running on.

roles
A directory with roles, each of which encapsulates logic for performing a specific task (for example, installing mongodb).

Roles in roles/:

detect_os/tasks
Define tasks for detecting the operating system on target hosts. It can be used to conditionally perform other tasks depending on the OS.

discover_load/tasks
Tasks for collecting information about the server load (CPU, memory).

dns/tasks
Configuring the DNS name of mongodb server for client.

firewall/tasks
Firewall management (iptables): opening ports, configuring rules.

mongo_install
Installing and configuring MongoDB (adding repositories, configuring the server).

routing_setup
Network routing settings (static routes, routing tables).

README.md
Project documentation: description of the structure, launch instructions, dependencies.

main_playbook.yml
The main playbook that includes roles or tasks to complete.

## **Quick Start**

```bash
# Clone repository
git clone https://github.com/Andrrew20/mongo_with_ansible.git
cd mongodb-ansible

# Install dependencies
# Python3 must be installed
pip install ansible

# Deploy infrastructure
ansible-playbook -i inventory/hosts.ini main_playbook.yml
```

### **Variables (`group_vars/all.yml`)**

````yaml
mongodb:
  version: "{your_version}"
  port: {your port(default: 27017)}
  dns_name: "mongodb.local" #mongodb-servername



### **Workflow**

1. **Load Analysis** - Identifies most loaded server for gateway
2. **Gateway Setup** - Configures NAT and IP forwarding
3. **MongoDB Deployment** - Installs and configures database
4. **Firewall Rules** - Restricts access to MongoDB
5. **DNS Update** - Adds `mongodb.local` to hosts

### **Troubleshooting**

```bash
# Check MongoDB status
ansible mongodb_servers -i inventory.ini -m shell -a "systemctl status mongod"
````
