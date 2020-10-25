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

Inventory file contains the list of all the servers which you want to perform some tasks on them.

---
## Part 04 (Ansible Playbooks)

A playbook is a single yaml file containing a set of plays. A play defines a set of activities or tasks to be run on a single or a group of hosts. Task examples:
- Execute a command
- Run a script
- Install a package
- Shutdown/Restart

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
# Simple Ansible Playbook
-
    name: Test1
    # Whatever host you choose must be added to Inventory File
    hosts: localhost
    tasks:
        - name: Execute command 'date'
          command: date
        - name: Execute script on server
          command: test_script.sh
-
    name: Test2
    hosts: localhost
    tasks:
        - name: Install web service
          yum:
            name: httpd
            state: present
        - name: Start web service
          services:
            name: httpd
            state: started
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
## Part 06 (References)

1. [Ansible Documentation](https://docs.ansible.com/)
2. [YAML](https://yaml.org/)