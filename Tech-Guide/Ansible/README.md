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
    - Detect and maintain appropriate patch levels (Increase the security of the systems)
2. Operation and Process
    - Increase efficiency and consistency
    - Automate manual tasks
    - Minimize human errors
3. Performance and Availability
    - Gain understanding into your workloads
    - Understand dependencies
    - Anticipate capacity requirements
    - Auto scale seamlessly

### Configuration Management Tools
- Chef (**Pull Method**: Pull configurations from different servers)
- Puppet (**Pull Method**: Pull configurations from different servers)
- Ansible (**Push Method**: Push configuration to different servers)

1. Availability

|DevOps Tools|Availability (in case of server failure)|
|------------|----------------------------------------|
|Chef|Backup Server|
|Puppet|Alternative Master|
|Ansible|Secondary Instance|

2. Configuration Language

|DevOps Tools|Configuration Language|Learning Level|
|------------|----------------------|--------------|
|Chef|Ruby DSL|Difficult|
|Puppet|Ruby, Puppet DSL, Embedded Ruby (ERB), DSL|Difficult|
|Ansible|Python, Yaml|Simple|

3. Setup and Installation

|DevOps Tools|Architecture|Ease of Setup and Installation|
|------------|------------|------------------------------|
|Chef|Master-Agent|Difficult and complex due to Chef workstation|
|Puppet|Master-Agent|Difficult due to certificate signing between master and agent|
|Ansible|Only Master (Agentless)|Easy|

4. Ease of Management

|DevOps Tools|Configuration|Ease of Management|
|------------|------------|-------------------|
|Chef|Pull|Difficult|
|Puppet|Pull|Difficult|
|Ansible|Push and Pull|Easy|

5. Scalability

|DevOps Tools|Scalability|
|------------|------------|
|Chef|High|
|Puppet|High|
|Ansible|Very High|

6. Interoperability

|DevOps Tools|Interoperability|
|------------|----------------|
|Chef|Chef Server should be on Linux/Unix; Workstation and Chef Client support Windows|
|Puppet|Puppet Master should be on Linux/Unix; Puppet Agent or Client supports Windows|
|Ansible|Ansible Server should be on Linux/Unix; Client machines support Windows|

7. Pricing

|DevOps Tools|Pricing|Pricing|
|------------|-------|-------|
|Chef|High|USD 13700/year for up to 100 nodes|
|Puppet|Medium|USD 11200-19900/year for up to 100 nodes|
|Ansible|Low|USD 10000/year for up to 100 nodes|

8. Tool Capabilities

- Chef
  - Continuous delivery with automated workflow
  - Compliance and security management
  - Infrastructure automation
- Puppet
  - Orchestration
  - Automated provisioning
  - Code and node management
  - Configuration automation
  - Simple visualization and reporting
  - High transparency
  - Role-based access control
- Ansible
  - Simple orchestration
  - Streamlined provisioning
  - Continuous delivery with automated workflow
  - App deployment
  - Security and compliance integration into automated processes

---
## Part 02 (Ansible Basics)

Features of Ansible:
1. It's agentless, so you don't need to install and manage an agent on the servers you need to run ansible on.
2. It uses SSH to establish secure connections.
3. It follows push bases architecture.
4. It is built on top of python, so it has a lot of functionalities of python built-in.

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
3. Fact Check: `ansible dev -m setup` or `ansible -m setup dev`
4. Download and run your personal playbook on your local machine: `ansible-pull -U URL`

Inventory Management Deep Dive:
- Patterns: `dev[1-4].example.com` or `dev[h-m].example.com` or `dev[1:4].example.com` or `dev[h:m].example.com`
- Privilege Escalation / Various levels of access
  - Host Variables: `dev.example.com ansible_user=john`
  - PrivEsc:
    - `become`: Allows you to force privilege escalation. *This Command is equivalent to ansible_sudo or ansible_su*
    - `become_method`: Allows you to set privilege escalation method.
    - `become_user`: Allows you to set the user you become through privilege escalation. *This Command is equivalent to ansible_sudo_user or ansible_su_user*
    - `become_pass`: Allows you to set the privilege escalation password. *This Command is equivalent to ansible_sudo_pass or ansible_su_pass*
    - `become_flags`: Allows you to use specific flags for the tasks or role. One common use is to change the user to nobody when the shell is set to nologin.
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

![Ansible Playbook](Images/ansible-playbook.png)

All Ansible playbooks are written in YAML.
- YAML: YAML Ain't Markup Language
- YAML is a human friendly data serialization standard for all programming languages.
- YAML files should end in .yaml or .yml
- YAML is case sensitive.
- YAML does not allow the use of tabs. Spaces are used instead, as tabs are not universally supported.

YAML Tutorial:
```yaml
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
```

Example:
```yaml
# Simple Ansible Playbook
# Whatever host you choose must be added to Inventory File
- hosts: ALL
  tasks:
  - name: This is a basic connectivity test
    ping:
```
Example:
```yaml
- name: Test1
  hosts: localhost
  tasks:
      - name: Execute command 'date'
        command: date
      - name: Execute script on server
        script: test_script.sh
- name: Test2
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
```

You can see the list of all modules using **`ansible-doc -l`** command on your terminal.

Running an Ansible playbook is quite simple. You can use `ansible-playbook FILE.yml` command to execute your Ansible playbook, or you can get help from `ansible-playbook --help` command in your terminal.

### Idempotency

An operation is idempotent if the result of performing it once is exactly the same as the result of performing it repeatedly without any intervening actions.

It's important to have idempotent machine, because the system needs to have a consistent state after running a playbook for multiple times.

---
## Part 05 (Ansible Modules)

Ansible modules categorized into various groups based on their functionality:
- System
  - User
  - Group
  - Hostname
  - Iptables
  - Lvg
  - Lvol
  - Make
  - Mount
  - Ping Timezone
  - Systemd
  - Service
  - etc.
- Commands
  - Command
  - Except
  - Raw
  - Script
  - Shell
  - etc.
- Files
  - Acl
  - Archive
  - Copy
  - File
  - Find
  - Lineinfile
  - Replace
  - Stat
  - Template
  - Unarchive
  - etc.
- Database
  - MongoDB
  - Microsoft SQL Server (mssql)
  - MySQL
  - PostgreSQL
  - ProxySQL
  - Vertica
  - etc.
- Cloud
  - Amazon
  - Atomic
  - Azure
  - Centrylink
  - Cloudscale
  - Cloudstack
  - Digital Ocean
  - Docker
  - Google
  - Linode
  - Openstack
  - Rackspace
  - Smartos
  - Softlayer
  - VMware
  - etc.
- Windows
  - Win_copy
  - Win_command
  - Win_domain
  - Win_file
  - Win_iis_website
  - Win_msg
  - Win_msi
  - Win_package
  - Win_ping
  - Win_path
  - Win_robocopy
  - Win_regedit
  - Win_shell
  - Win_service
  - Win_user
  - etc.
- etc.

Deep-Dive into some useful modules:
- Command: Executes a command on a remote node
- Script: Run a local script on a remote node after transferring it
- Service: Manage services on a system such as start, stop, or restart
- Lineinfile: Search for a line in a file and replace it or add it if it doesn't exist.

---
## Part 06 (Ansible Concepts)

### Variables

The format we are using to define variables called **Jinja2 Template**.

If the variable is in the middle of a sentence, a single quote around the double curly braces are not required.

Example of defining variables:
```yaml
- name: add DNS server to resolv.conf
  hosts: localhost
  vars:
    dns_server: 8.8.8.8
  tasks:
    - lineinfile:
      path: /etc/resolv.conf
      line: 'nameserver {{ dns_server }}'
```

Ansible has 3 types of variables:
1. Dictionary
2. List
3. Ansible Unsafe Text

Example of variable types:
```yaml
- hosts: dev
  gather_facts: yes
  tasks:
  - debug:
    msg:
    - "Type of ansible_cmdline variable is: {{ ansible_cmdline | type_debug }}"
    - "Type of ansible_all_ipv4_addresses variable is: {{ ansible_all_ipv4_addresses | type_debug }}"
    - "Type of ansible_architecture variable is: {{ ansible_architecture | type_debug }}"
```

### Conditionals

Conditional statements in Ansible is like programming languages. You can use *and*, *or*, *==*, *!=*, *when*, and etc.

Example 1:
```yaml
- name: Start Services
  hosts: all_servers
  tasks:
    - service: name=mysql state=started
      when: ansible_host == "db.company.com"
    
    - service: name=httpd state=started
      when: ansible_connection == "web1.company.com" or ansible_connection "web2.company.com"
```

Example 2:
```yaml
- name: Check status of service and email if it's down
  hosts: localhost
  tasks:
    - command: service https status
      register: command_output

    - mail:
        to: Admins <admin@company.com>
        subject: Service Alert
        body: 'Service {{ ansible_hostname }} is down!'
      when: command_output.stdout.find('down') != -1
```

### Loops

Example:
```yaml
- name: Install Packages
  hosts: localhost
  tasks:
    - yum: name='{{ item }}' state=present
      with_items:
        - httpd
        - wget
        - curl
        - gcc
        - make
        - htop
        - python3-pip
```

### Roles

There's a large playbook file with hundreds lines of code. It's frustrating to maintain, change, and support it.
We must cut this huge file into smaller files and use a single master playbook to control these files using `- include <playbook name>` in the master playbook.

Example:
```yaml
# setup_applications_debian.yml (2000 lines of code)
# we must separate it into smaller files.
# 1. update_servers.yml (500 lines of code)
# 2. install_dependencies.yml (500 lines of code)
# 3. configure_webservers.yml (500 lines of code)
# 4. start_applications.yml (500 lines of code)

# The master configuration file is:
- include update_servers.yml
- include install_dependencies.yml
- include configure_webservers.yml
- include start_applications.yml
```

Note that, we can include tasks and variables in a different files as well.

It is recommended that we make a *roles* directory inside our Ansible directory and follow *Role Directory Structure*, because ansible can understand and extract every data that it needs to complete the tasks.

```yaml
- name: Set Firewall Configuration
  hosts: web
  roles:
    - webservers
```

Sample Role Directory Structure:

```
inventory.txt
master_configuration.yml
roles/
    common/
        tasks/
            main.yml
        handlers/
        library/
        files/
        templates/
        vars/
            main.yml
        defaults/
        meta/
    webservers/
        files/
        templates/
        tasks/
            main.yml
        handlers/
        vars/
            main.yml
        default/
        meta/
```
---
## Part 07 (Advanced Topics)

### Preparing Windows Server

- Ansible Control Machine can only be Linux.
- Windows machines can be targets of Ansible and thus be part of automation.
- Ansible connects to a windows machine using **winrm**.
- Requirements:
  - *pywinrm* module should be installed on the Ansible Control Machine. `pip install pywinrm`
  - Setup **winrm** with a powershell script provided and recommended by Ansible. (*ConfigureRemotingForAnsible.ps1*)
  - Different modes of authentication: Basic/Certificate/Kerberos/NTLM/CredSSP

### Ansible-Galaxy

[Galaxy](https://galaxy.ansible.com) is your hub for finding, reusing and sharing the best Ansible content.

### Patterns

You can use wildcards in your Ansible-Playbook for the hosts you choose. Examples:
- `Host1, Host2, Host3`
- `Group1, Host1`
- `Host*`
- `*.company.com`
- `192.168.100.*`
- `webserver[2:5]`
- `database[3:]`
- `firewall[:4]`
- `~(web|db).*\.company\.com`

### Dynamic Inventory

It's not necessary to always define a inventory information in these files. Because the *inventory.txt* is a static file and you will have to change that manually. If you need to integrate ansible with any other source of inventory in your environment, then you will need to make this inventory dynamic.

Ansible supports having dynamic inventory files. You just need to specify your script instead of your inventory file. You can check some of the scripts at the Ansible documentation site.

### Developing Custom Modules

You need to write a Python program and place it in the modules directory on your server. The program though has to be written in a particular format. You can get the template to start with from the Ansible documentation site.

### Managing Return Values using Register

- Common type of return values
  - `backup_file`: For those modules that implement `backup=no|yes` when manipulating files, a path to the backup file created.
  - `changed`: A boolean indicating if the task had to make changes.
  - `failed`: A boolean that indicates if the task was failed or not.
  - `invocation`: Information on how to module was invoked.
  - `msg`: A string with a generic message relayed to the user.
  - `stderr`: Some modules execute command line utilities or are geared for executing commands directly (raw, shell, command, etc.), this field contains the error output of these utilities.
  - `stderr_lines`: When `stderr` is returned we also always provide this field which is a list of strings, one item per line from the original.
  - `stdout`: Some modules execute command line utilities or are geared for executing commands directly (raw, shell, command, etc.), this field contains the normal output of these utilities.
  - `stdout_lines`: When `stdout` is returned, Ansible always provides a list of strings, each containing one item per line from the original output.

---
## Part 08 (Ansible Terminology)

- Control Node: Machine with Ansible installed
- Managed Node: Machines that you manage with Ansible
- Inventory: List of managed nodes, default file location is `/etc/ansible/hosts`
- Modules: Unit of code that Ansible executes
- Tasks: Unit of action in Ansible
- Playbooks: Ordered list of tasks

---
## Part 09 (References)

1. [Ansible Documentation](https://docs.ansible.com/)
2. [YAML](https://yaml.org/)
3. [Configuration Management Tools Comparison](https://www.veritis.com/blog/chef-vs-puppet-vs-ansible-comparison-of-devops-management-tools/)