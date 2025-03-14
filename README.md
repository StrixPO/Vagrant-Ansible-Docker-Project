# Automated DevOps Environment with Vagrant, Ansible, and Docker

## Project Overview
This project demonstrates a fully automated multi-node environment using Vagrant, Ansible, and Docker. The primary goal is to provision multiple Linux servers, configure them using Ansible playbooks, and deploy a simple Dockerized application across nodes. This serves as a foundational DevOps project to practice infrastructure automation and containerization.

## Technologies Used
- **Vagrant:** Automates the creation of virtual machines.
- **Ansible:** Automates configuration and package management.
- **Docker:** Containerizes the application.
- **Docker Compose:** Manages multi-container applications.
- **Linux (Ubuntu):** Operating system for virtual machines.
- **Python 3:** Required for Ansible and application execution.
- **Bash Scripting:** Automates common tasks.

## Project Structure
```
devops_practice/
├── ansible/ (myhost, playbook.yml)
├── docker/ (app.py, docker-compose.yml, Dockerfile, requirements.txt)
├── hosts
├── Vagrantfile
├── README.md
```

## Project Setup
### Prerequisites
- Vagrant installed on your local machine.
- VirtualBox or any compatible virtualization provider.
- Docker installed on VM instances.
- Python 3 and Ansible installed on control node.

### Provisioning Servers
1. Start the Vagrant environment:
   ```bash
   vagrant up
   ```

2. SSH into the control node:
   ```bash
   vagrant ssh control
   ```

3. Update host file and distribute SSH keys:
   ```bash
   cd /vagrant/
   cp hosts /etc/hosts
   ssh-keygen
   ssh-copy-id node1 && ssh-copy-id node2 && ssh-copy-id node3
   ```
   > Password for SSH: `vagrant`

### Configure Nodes with Ansible
1. Change directory to Ansible files:
   ```bash
   cd /ansible/
   ```

2. Update packages on all nodes:
   ```bash
   ansible nodes -i myhost -m apt -a "update_cache=yes" --become
   ```

3. Install Python 3 on nodes:
   ```bash
   ansible nodes -i myhost -m apt -a "name=python3 state=present" --become
   ```

### Deploying the Docker Application
1. SSH into Node 1:
   ```bash
   ssh vagrant@node1
   ```

2. Navigate to the Docker directory and bring up the application:
   ```bash
   cd /vagrant/docker
   docker-compose up
   ```

### Testing the Deployment
1. From the control node, verify the application:
   ```bash
   curl node1:5000
   ```

## Project Highlights
- Automated multi-node environment setup using Vagrant.
- Efficient configuration management via Ansible.
- Containerization and application deployment with Docker and Docker Compose.
- Modular and maintainable project structure.

## Future Enhancements
- Implementing Docker Swarm for multi-node orchestration.
- Automating Docker Swarm setup with Ansible.
- Integrating CI/CD pipeline using GitHub Actions.

## License
This project is licensed under the MIT License.

