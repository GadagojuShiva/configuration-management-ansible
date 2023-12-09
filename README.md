<img src="https://img.shields.io/badge/Ansible-Configuration_Management-yellow?style=flat&logo=ansible&logoColor=black" alt="Ansible Configuration Management" width="1000"/>


## Overview

Configuration management tool used by <strong> DevOps </strong> engineers to automate the process of configuring servers. It facilitates tasks such as patching, upgrading, and installing software on multiple servers simultaneously.

## What is Ansible?

Ansible is a configuration management tool that operates on a push mechanism. Unlike some other tools like <strong> Puppet </strong>  and <strong>Chef </strong>, <strong>Ansible</strong> is agentless, supports both Windows and Linux environments, and uses YAML as its configuration language.

### Key Features of Ansible

1. **Push Mechanism:**
   Ansible uses a <strong>push mechanism</strong>, allowing you to write playbooks on your local machine and push the configurations to multiple servers.

2. **Agentless:**
   Ansible doesn't require agents to be installed on target servers. It uses <strong>passwordless</strong> authentication, making it easy to connect to servers from your local machine.

3. **Cross-Platform Support:**
   Ansible supports both Windows and Linux environments, making it versatile for managing diverse server infrastructures.

4. **YAML Language:**
   Configuration in Ansible is written in <strong>YAML</strong>, a human-readable and globally recognized language, making it easy to understand and write playbooks.

## Getting Started with Ansible

### Installation

To install Ansible, follow these steps:

1. Launch two EC2 instances named `ansible-server` and `target-server`
2. SSH into the 'ansible-server' and execute the following commands:
   ```bash
   sudo apt-get update
   sudo apt install ansible
   ```
## Passwordless Authentication Setup
1. Generate an SSH key pair on the `ansible-server` using the following command:
   ```bash
   ssh-keygen 
   ```
2. ssh into the `target-server` and execute  the following commands:
   ```bash
   ssh-keygen
   ```
3. Copy the public key which is generated on `ansible-server`and paste it into the `authorized_keys` file on the `target-server.`
   
4. Now exit from the target server come back to `ansible-server` and Create an inventory file named `inventory` with the private IPs of target servers. 
   For example:

   ```bash
   private_ip_of_target_server -> (eg: 172.168.0.0)
   ```
5. Run an adhoc command to create a file on the target server:
   ```bash
   ansible -i inventory -m "shell" -a "touch devops"
   ```
- Now you succefully created a file on the target server by using the adhoc command

## Ansible Playbooks

1. <strong>Task:</strong> Write an Ansible playbook to install and start an HTTP server on target servers. 
2. Example playbook `(my-first-playbook.yml)`:

   ```yml
   ---
   - name: installing and starting httpd servers
     hosts: all
     become: true

     tasks:
       - name: installing httpd
         apt:
            name: httpd
            state: present

      - name: starting httpd
        service:
            name: httpd
            state: started
            enabled: true
   ```
3. Run the playbook:
   ```bash
   ansible-playbook -i inventory my-first-playbook.yml
   ```
- This will successfully install the  httpd and start the  server on the target machine
## Ansible Roles

Ansible roles help organize and modularize tasks.
 
## Directory Structure

The directory structure for this Ansible role follows the standard conventions
```bash
   my_ansible_role/
            |-- defaults/
            |-- files/
            |-- handlers/
            |-- meta/
            |-- tasks/
            |-- templates/
            |-- tests/
            |-- vars/
            |-- .github/
            |-- LICENSE
            |-- README.md
```
1. `tasks/`

     - `tasks/` directory contains the main list of tasks that the role will perform. These tasks are defined in YAML format in separate files.

2. `handlers/`

   - The `handlers/` directory contains handlers, which are special tasks that can be triggered by other tasks. Handlers are defined in separate files.

3. `files/`

   - The `files/` directory contains files that need to be transferred to the remote machine.

4. `templates/`

   - The `templates/` directory contains Jinja2 templates, which are used to generate configuration files.

5.  `vars/`

       - The `vars/` directory contains variable files. These files may define variables used in the role tasks.

6. `defaults/`

   - The `defaults/` directory contains default variable values for the role. These variables are used if no other values are provided.

7. `meta/`

   - The `meta/` directory contains metadata about the role, such as dependencies. This is where you can specify role dependencies.
  
## Example Role Usage
   For a complex task, like setting up a Kubernetes cluster with a master and worker nodes, Ansible roles can efficiently manage the process.
## Conclusion
Ansible is a flexible and powerful configuration management tool that simplifies the automation of server configurations. By using playbooks, adhoc commands, and roles, DevOps engineers can efficiently manage and scale their infrastructure.
