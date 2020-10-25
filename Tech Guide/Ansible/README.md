# Ansible

## Part 01 (Introduction)

Ansible is a simple, powerful, cross-platform, and agentless IT automation tool that everyone can use for their needs.

Why Ansible:
- Provisioning
- Configuration Management
- Continuous Delivery
- Application Deployment
- Security Compliance

---
## Part 02 (Ansible Basics)

---
## Part 03 (Ansible Inventory Files)

---
## Part 04 (Ansible Playbooks)

All Ansible playbooks are written in YAML.
- YAML: YAML Ain't Markup Language
- YAML is a human friendly data serialization
  standard for all programming languages.

A playbook is a single yaml file containing a set of plays. A play defines a set of activities or tasks to be run on a single or a group of hosts. Task examples:
- Execute a command
- Run a script
- Install a package
- Shutdown/Restart

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