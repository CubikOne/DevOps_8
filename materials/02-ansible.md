# Ansible


**Ansible** is a tool for automating remote node configuration. Its main feature is the absence of any agent-server structure. Instead, Ansible can be installed on only one machine (which is not necessarily any manager or major important node in the network topology), it is only important to be able to establish a connection. **Ansible** connects via **ssh** and executes the usual bash commands. In addition, **Ansible** collects back information or *facts* about remote machines that allow **Ansible** to not only change the configuration state of a node, but also to store information about changes in that state and ensure that that state remains unchanged by the configurator (since the node itself naturally remains "unprotected" for manual configuration).


To get started with **Ansible**, you will need to create an inventory file containing the addresses or domain names of all the remote hosts that require configuration. In the case of Vagrant virtual machines, these will be the ip addresses on the internal Vagrant network.


Now, to run some action or module using ansible on all connected machines, the following command must be executed:


`ansible all --key-file <path to ssh key> -i <path to inventory file> -m <model name>` .


The official documentation contains information about all available modules. Use `man ansible` to get familiar with the other keys, some of which you may need, for example, to run commands with root privileges.


Many commands for ansible can be combined into a single yml-file - a playbook. The simplest playbook has the following structure:


```yml
- hosts: <group>               # by default - all
 <key №1>: <value №1>      # additional keys and arguments similar to ansible
 <key №2>: <value №2>
 <key №2>: <value №3>
 tasks:                        # task list
   - name: <task name №1>     # task name
     <module name>:             # module and its parameters
       <parameter №1>: <value №1>
       <parameter №2>: <value №2>
       <parameter №3>: <value №3>
   - name: <task name №2>     # task name
     <module name>:             # module and its parameters
       <parameter №1>: <value №1>
       <parameter №2>: <value №2>
       <parameter №3>: <value №3>
   <...>
```


To run the playbook, enter the command:


`ansible-playbook <playbook name>`


Playbooks **Ansible** support the creation of roles. Roles allow you to logically allocate commands to individual files located in a specific directory tree structure:


```
./roles/
----role1/
--------tasks/
------------main.yml            //tasks of the first role
----role2/
--------tasks/
------------main.yml            //tasks of the second role
----role3/
--------tasks/
------------main.yml            //tasks of the third role
```


In addition to tasks, you can set your templates, variables (vars), and other useful things in the roles.
