# Ansible

## Part 01 (Introduction)

Ansible is a simple, powerful, cross-platform, and agentless IT automation tool that everyone can use for their needs.

Why Ansible:
- Provisioning
- Configuration Management
- Continuous Delivery
- Application Deployment
- Security Compliance

Benefits of Automation:
1. Infrastructure Configuration
    - Improve overall infrastructure performance and compliance
    - Maintain consistent configuration
    - Detect and maintain approprate patch levels (Increase the security of the systems)
2. Operation and Process
    - Increase efficiency and consistency
    - Automate manual tasks
    - Minimize human errors
3. Performance and Availability
    - Gain understanding into your workloads
    - Understand dependencies
    - Anticipate capacity requirements
    - Auto scale seamlessly

Configuration Management Tools:
- Ansible (**Push Method**: Push configuration to different servers)
- Chef (**Pull Method**: Pull configurations from different servers)
- Puppet (**Pull Method**: Pull configurations from different servers)

---
## Part 02 (Ansible Basics)

Features of Ansible:
1. It's agentless, so you don't need to install and manage an agent on the servers you need to run ansible on.
2. It uses SSH to establish secure connections.
3. It follows push bases architecture.
4. It is built on top of python, so it has a lot of functionalities of python in-built.

Setup Password-less SSH Connection:
- Public-Private key encryption
- Generate keys using `ssh-keygen` command
- Transfer keys using `ssh-copy-id` command: `ssh-copy-id USER@SERVER_IP`
- Validate

---
## Part 03 (Ansible Inventory Files)

Inventory file (host file) contains the list of all the servers which you want to perform some tasks on them.

It's good to separate servers for their specific purposes on different groups, and add all servers in a single group so that you can access all of them at once.

Example of `/etc/ansible/hosts`:
```
[DEV]
IP/HOST/localhost
IP/HOST/localhost

[PROD]
IP/HOST/localhost
IP/HOST/localhost

[MAIL]
IP/HOST/localhost
IP/HOST/localhost

[ALL]
IP/HOST/localhost
IP/HOST/localhost
IP/HOST/localhost
```

Use simple modules to run commands on the servers:
1. `ansible -m ping all` or `ansible all -m ping`
2. `ansible -m shell -a 'free -m' all` or `ansible all -m shell -a 'free -m'`

Inventory Management Deep Dive:
- Patterns: `dev[1-4].example.com` or `dev[h-m].example.com` or `dev[1:4].example.com` or `dev[h:m].example.com`
- Privilege Escalation / Various levels of access
  - Host Variables: `dev.example.com ansible_user=john`
- Connections
  - Windows / Linux: `dev.example.com ansible_connection=winrm` or `prod.example.com ansible_connection=ssh` or `mail.example.com ansible_connection=localhost`
  - Different port for SSH: `dev.example.com:2222` or `dev.example.com ansible_port=2345`
  - Access locally: `dev.example.com ansible_connection=local` or `dev.example.com ansible_connection=ssh`
  - Open a port for specific program: `dev.example.com http_port=80`
  - SSH Password: `dev.example.com ansible_ssh_pass=PASSWORD`
    - There's a better way for managing password instead of storing in a plain text file which is not recommended in production environment. **Ansible Vault** stores passwords in an encrypted format. Check `ansible-vault --help` command.
    - The best way to connect servers with each other is sharing SSH-Keys.
- Group Variables (Assign variable for specific group once)
  ```
  [DEV:vars]
  proxy_server=proxy.example.com
  ```
- Supergroups: contains multiple groups
  ```
  [NAME_OF_SUPERGROUP:children]
  DEV
  PROD

  [all_servers:children]
  DEV
  PROD
  MAIL
  ```
- Using non-default inventory file: `ansible-playbook -i /ADDRESS/OF/hosts /ADDRESS/OF/FILE.yml`

---
## Part 04 (Ansible Playbooks)

A playbook is a single yaml file containing a set of plays. A play defines a set of activities or tasks to be run on a single or a group of hosts. Task examples:
- Execute a command
- Run a script
- Install a package
- Shutdown/Restart
- Deploy number of VMs on cloud
- Provision Storage to all VMs
- Setup network configuration
- Setup cluster configuration
- Configure Web-Server/Database
- Setup Load-Balancing between web server VMs
- Setup monitoring components
- Update CMDB database with new VM information
- etc.

All Ansible playbooks are written in YAML.
- YAML: YAML Ain't Markup Language
- YAML is a human friendly data serialization standard for all programming languages.
- YAML files should end in .yaml or .yml
- YAML is case sensitive.
- YAML does not allow the use of tabs. Spaces are used instead, as tabs are not universally supported.

YAML Tutorial:
```yaml
---
# Yaml file starts with 3 dashes/hyphens
# Scalar/Variable
Title: "This is a tutorial"
Page: 15
Price: 12.35
For_sale: yes

# Sequence/List
- toy
- animal

# Nested Sequence/List
- toy
 - car
 - plane
 - doll
- animal
 - cat
 - dog 

# Mapping (Exactly like Key:Value pair)
Drink: cold_drink

# Mapping with a sequence
Cold_drinks:
 - Coke
 - Pepsi
  - Pepsi Max
 - Sprite
Hot_drinks:
 - Tea
 - Coffee
 - Milk

# Flow Collection (Exactly like Key:Values)
fruit: ['Apple', 'Orange', 'Mango']

# Yaml file ends with 3 dots
...
```

Example:
```yaml
---
# Simple Ansible Playbook
# Whatever host you choose must be added to Inventory File
- hosts: ALL
  tasks:
  - name: This is a basic connectivity test
    ping:
...
```
Example:
```yaml
---
-
  name: Test1
  hosts: localhost
  tasks:
      - name: Execute command 'date'
        command: date
      - name: Execute script on server
        script: test_script.sh
-
  name: Test2
  hosts: localhost
  tasks:
      - name: Install web service
        yum:
          name: httpd
          state: present
      - name: Start web service
        service:
          name: httpd
          state: started
# command, script, yum, and service are some of Ansible's modules.
...
```

You can see the list of all modules using **`ansible-doc -l`** command on your terminal.

Running an Ansible playbook is quite simple. You can use `ansible-playbook FILE.yml` command to execute your Ansible playbook, or you can get help from `ansible-playbook --help` command in your terminal.

---
## Part 05 (Ansible Concepts)

### Variables

### Conditionals

### Loops

### Roles

---
## Part 06 (Ansible Terminology)

- Control Node: Machine with Ansible installed
- Managed Node: Machines that you manage with Ansible
- Inventory: List of managed nodes, default file location is `/etc/ansible/hosts`
- Modules: Unit of code that Ansible executes
- Tasks: Unit of action in Ansible
- Playbooks: Ordered list of tasks

---
## Part 07 (References)

1. [Ansible Documentation](https://docs.ansible.com/)
2. [YAML](https://yaml.org/)