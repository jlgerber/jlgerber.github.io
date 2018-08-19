### Ansible
Ansible is an configuration management framework. Unlike other popular frameworks, it does not run a persistent daemon on each target machine, making it more suitable for deployments than state management. However, it also does not need to be installed on target machines; only on the server from which you will be running playbooks. This server is commonly referred to as the *Ansible Control Server*, although server is a misnomer, as often a lowly laptop plays the role.

The only thing which Ansible truly needs on the target machines are an ssh server and python2.7.

#### Add hoc
Ansible may be run from the command line, specifying an inventory file, a host or hosts from the file, and a module to run directly. EGs. Inventory files are covered later, but at their simplest, they are just a list of machines to operate on.

For example, to ping all the machines in the inventory file:
```
ansible  -i inventory -m ping all
```
And to gather facts on all of the machines:
```
ansible  -i inventory -m setup all
```
###### Passing arguments to modules
You can pass arguments to modules using the ```-a``` flag. Arguments are formated as ```key=value``` pairs, space seprated and all encased in quotes. EG (assuming there is a db2 in the inventory):
```
ansible -i inventory -m setup -a 'filter=ansible_*' db1
```

### Inventory Files
Inventory files are used to describe and group the available machines upon which we would like ansible to run.

#### Inventory File Features
- groups
- groups of groups
- behavior parameters
- variables
- support for multiple files
- static/dynamic

###### Machines
Machines may be set at the top of the inventory, without any grouping. These machines should either be fully qualified, or ahve their ip addresses specified explicitly (which we will see later).
```
web1.company.com
web2.company.com
```
###### Groups
Target machines may be grouped together. A group is defined as a list of machine names, one per line, under a group name, encased in *[]*.

For Example,
```
[dbs]
db1.company.com
db2.company.com
```

defines two target machines (db1.company.com and db2.company.com) under a db group. You may refer to the *db* group in lieu of a specific host, in a play's *hosts:* key.

##### Groups of Groups

You can define a group of groups by using the *:children* modifier when defining a parent group name. For example:
```
[dbs]
db1
db2

[webs]
web1
web2

[datacenter:children]
dbs
webs
```
#### Behavior Parameters
Behavior parameters can be set either on individual servers like so:
```
db1 ansible_ssh_user=jlgerber ansible_ssh_pass=foobar
```
or declared for groups:
```
[dbs:var]
ansible_ssh_user=jgerber
ansible_ssh_pass=foobar
```

If you are setting up machines on vms, with, say, vagrant, you wont necesarily have dns set up. You can specify ip addresses for machines like so:
```
web1 ansible_ssh_host=192.168.33.20
db1 ansible_ssh_host=192.168.33.30

[webservers]
web1

[dbservers]
db1
```

#### Plays, Tasks, Handlers and Playbooks
Playbooks contain plays.
Plays contain tasks
Tasks call modules.

Tasks run sequentially

Handlers are triggered by tasksm adn are run once, at the end of plays.

##### Plays
Each play maps hosts to tasks. A play may have multiple tasks. Plays are generally stored in playbooks. A play is represented in a playbook as a yaml block which starts with a host and defines a block of tasks:

```
- hosts: webservers
  remote_user: root
  tasks:
    - name: Install Apache
      yum: name=httpd state=present
    - name: Start Apache
      service: name=httpd state=started
```

Plays may declare a number of additional properties. They may declare variables directly:
```
- hosts: webservers
  sudo: yes
  sudo_user: special_user
  gather_facts: no
  vars:
    git_repo: https://github.com/jlgerber/foo.git
    httpd_port: 80
    db_name: yaams
```
##### Tasks
Tasks are provided as a list of dictionaries under the ```tasks``` key.
Each task starts with a ```name:``` parameter, which provides a human readable description of the task.
Following the name, the module name appears, along with one or more <key>=<value> pairs configuring the module. For example:
```
- name: Install Apache
  yum: name=http state=present
```

##### Modules
Each task executes a module. Modules are documented on ```docs.ansible.com``` and may be introspected on the command line.
To get a list of modules:
```
ansible-doc -l
```
To get information on a module's parameters:
```
ansible-doc <modulename>
eg
ansible-doc git
```

##### Handlers
Handlers look like tasks. They each have a name and a module invocation:

```
handlers:
- name: restart apache
  service: name=httpd state=restarted
```

Handlers are triggered using a ```notify``` directive in a task. Ansible matches the notification string with the name of the handler. For example, a task which would ultimately trigger the aforementioned handler would look like this:
```
tasks:
  - name: write apache config
    template: src=srv/httpd.j2 dest=/etc/httpd.conf
    notify:
      - restart apache
```
##### Playbooks
Playbooks contain one or more plays:
```
---
- hosts: webservers
  remote_user: root
  tasks:
    - name: Install Apache
      yum: name=httpd state=present
    - name: Start Apache
      service: name=httpd state=started

- hosts: dbservers
  remote_user: root
  tasks:
    - name: Install mariadb
      yum: name=mariadb-server state=present
    - name: Start mariadb
      service: name=mariadb state=started
```
###### Executing Playbooks
A playbook is executed using the ```ansible-playbook``` command like so:
```
ansible-playbook [-i inventory] <playbook.yml>
```
If the inventory is identified in the ansible.cfg or the environment then you do not have to provide an inventory via the ```-i <inventory>``` flag. To set the inventory in the cfg file, add the following:
```
[defauts]
hostfile = inventory
```
Where **inventory** is the name of the inventory file.

Alternatively, the hostfile may be set in the environment, like so (assuming we are in bash):
```
export ANSIBLE_HOSTFILE=inventory
```
###### Dealing with Failures
If a host fails somewhere in a playbook, it is pulled out of the lineup for subsequent plays and tasks. Ansilbe will report it in the recap, along with a flag to supply ansible-playbook in order to retry the failed hosts.
```
PLAY RECAP***********************************
to retry use:--limit@/home/vagrant/ping.retry
```
 Make your changes and re-execute with the flag.
 ```
 ansible-playbook myplaybook.yml --limit@/home/vagrant/ping.retry
 ```

#### Roles
A Role defines a specicic set of tasks to be caried out for a specific subset of the inventory.

##### Executing Roles
A role is executed from a playbook, under the roles key. Here is an example playbook's contents which executes on the webservers group, as defined in an inventory file:
```
---
- hosts: webservers
  sudo: yes
  gather_facts: no
  roles:
    - webserver
```

This playbook would be stored in the root of an ansible project.

#### Project Organization - Directory Based

A typical decent sized ansible project is made up of a series of directories, under a root:

```
<root>/
  <playbook>.yml
  inventory.yml
  ansible.cfg
  roles/
    <rolename>/
      tasks/
        main.yml
      handlers/
        main.yml
      templates/
        <name>.j2
        ...
      vars/
        main.yml
```
##### playbooks
You can have multiple playbooks but only one playbook will be executed, per your ```ansible-playbook <playbook>.yml``` command. You can aggregate playbooks via the ```include``` directive. For example, given a master playbook, and a couple of secondary playbooks( webserver.yml and dbserver.yml ):

```
---
- hosts: dbservers:webservers
  sudo: yes
  gather_facts: no
  roles:
    - server-common

- include: webserver.yml
- include: dbserver.yml
```

##### tasks subdirectories

The handlers, templates, and vars directories under ```roles/<rolename>``` are optional; if you dont have any handlers, vars, or templates, you don't have to create them. Just create each directory when needed.

You can break up the tasks by creating additional yml files, and including them in the main.yml. For example, given an snmp.yml task file and a syslog.yml task file, the main.yml would look like:
```
---
- include: snmp.yml
- include: syslog.yml
```
#### Ansible Galaxy
Ansible hosts a [website](https://galaxy.ansible.com) where users upload and rate roles. It is [well documented here.](http://docs.ansible.com/ansible/latest/galaxy.html#ansible-galaxy).

Ansible galaxy roles may be installed locally using the ```ansible-galaxy``` command.
##### Commands
###### delete
The delete command requires that you first authenticate using the login command. Once authenticated you can remove a role from the Galaxy web site. You are only allowed to remove roles where you have access to the repository in GitHub.

###### import
The import command requires that you first authenticate using the login command. Once authenticated you can import any GitHub repository that you own or have been granted access.

###### info
Get more infomration about a given role:
```
ansible-galaxy info geerlinggut.elasticsearch
```

###### init
Use the init command to initialize the base structure of a new role, saving time on creating the various directories and main.yml files a role requires

```
ansible-galaxy init role_name
```
Calling init will produce the expand the template in the current directory. Issuing this command requires access to the internet by default,as the template is defined externally. Here is what will be created by default:
```
README.md
.travis.yml
defaults/
    main.yml
files/
handlers/
    main.yml
meta/
    main.yml
templates/
tests/
    inventory
    test.yml
vars/
    main.yml
```

###### install

You can install roles from the Galaxy server locally using this subcommand. EG:
```
ansible-galaxy install username.role_name
eg
ansible-galaxy install geerlingguy.elasticsearch
```
####### roles_path

By default, Ansible downloads roles to the path specified by the environment variable ANSIBLE_ROLES_PATH. This can be set to a series of directories (i.e. /etc/ansible/roles:~/.ansible/roles), in which case the first writable path will be used. When Ansible is first installed it defaults to /etc/ansible/roles, which requires root privileges.

You can override this by setting the environment variable in your session, defining roles_path in an ansible.cfg file, or by using the –roles-path option. The following provides an example of using –roles-path to install the role into the current working directory:
```
ansible-galaxy install --roles-path . geerlingguy.apache
```
####### specify a version
You can specify a version using the following syntax:
```
 ansible-galaxy install geerlingguy.apache,v1.0.0
```
It’s also possible to point directly to the git repository and specify a branch name or commit hash as the version. For example, the following will install a specific commit:
```
ansible-galaxy install git+https://github.com/geerlingguy/ansible-role-apache.git,0b7cd353c0250e87a26e0499e59e7fd265cc2f25
```
###### list
List installed roles.

###### login
Using the ```import```, ```delete``` and ```setup``` commands to manage your roles on the Galaxy website requires authentication, and the login command can be used to do just that. Before you can use the login command, you must create an account on the Galaxy website.

###### remove
Delete a role from somewhere in the role path.

###### search
Search for roles on Galaxy. There are a number of tags which modify the search behavior, including ```--author```, ```--galaxy-tags```, and ```--platforms```. Here is a search example:
```
 ansible-galaxy search elasticsearch --author geerlingguy
 ```
 The main weakness of this command is that one cannot sort by categories like one can on the website.

###### setup

#### Installation Notes
##### Upgrades on OS X
I was force to use the following command to upgrade ansible, as a straight ```pip install ansible --upgrade``` threw an exception.

```
sudo -H pip install --upgrade ansible --user python
```