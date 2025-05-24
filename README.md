# **MongoDB Deployment with Ansible**

![Ansible](https://img.shields.io/badge/ansible-2.10+-red.svg) ![MongoDB](https://img.shields.io/badge/mongodb-7.0-green.svg) ![License](https://img.shields.io/badge/license-MIT-blue.svg)

Automated deployment of MongoDB across hybrid Linux environments with intelligent gateway routing and access control.

## **Features**

- 🔒 **Secure deployment** - Firewall-restricted MongoDB access
- 🖥️ **Multi-OS support** - RHEL (Alt Linux, RedOS) and Debian (Astra Linux)

## **Quick Start**

```bash
# Clone repository
git clone https://github.com/Andrrew20/mongo_with_ansible.git
cd mongodb-ansible

# Install dependencies
pip install ansible

# Deploy infrastructure
ansible-playbook -i inventory/hosts.ini main_playbook.yml -v
```

### **Variables (`group_vars/all.yml`)**

````yaml
mongodb:
  version: "{your_version}"
  port: {your port(default: 27017)}
  dns_name: "mongodb.local" #mongodb-servername

## **Workflow**

1. **Load Analysis** - Identifies most loaded server for gateway
2. **Gateway Setup** - Configures NAT and IP forwarding
3. **MongoDB Deployment** - Installs and configures database
4. **Firewall Rules** - Restricts access to MongoDB
5. **DNS Update** - Adds `mongodb.local` to hosts

## **Troubleshooting**

```bash
# Check MongoDB status
ansible mongodb_servers -i inventory.ini -m shell -a "systemctl status mongod"
````
