### Ansible

Developers  ++ testing >>>> aproval qa >>> operations >>> Customers
* 2nd 
  * version controll system (vcs) >>> build package >> system test environment >> performance test environment  >> pre prepoduction environment
  
  live production Environment
* Devoloper to live >> Continues Deployement 
  Devoloper to prepdoduction >>> Continues Deliver >>Azure devops
  Devoper to Build packge code anlysis >>> Continues intigration >> jenkins

why devops

1. frequent smallar updates
2. Operations shifted left   
   
   vcs>>> git ...   
   Build code>> Maven..Ms build etc... Docker package >.build ..Kubernetes run package
   
   Infra provisioning>> Teraform to deploy application create servers for us to create infrastructure
   
   Configuration Management > > Ansible ()   
   CI-CD engines >> Jenkins & Azuredevops call other tools
   
   Monitoring>> elastic stockserver monitoring   
   Sre ..how google run devops   
   
   Secrity engineers>>> Dev scops ..SCA>> software composition anlysis ...SAST>>static application security test
   
### Understanding Ansible through out aplication

....story of toy craft

  Reverse Proxy...
  Webserver..
  Aplication Server...
  Database
  DB server
  app.netcore
  Bussiness Logic
  Aplication Servers  
 
....all the servers need to update frequently in every place

Deployment options
  
  * Manual ...Admin performs manual steps
  * shell scrpit ... Admins executes a shell script
  * Configuration Management

  ... shell sript problems
   
   * readbilitie less
   * giving same results might not be happend
   * Approch : Procdural
  
   
  ...Configuration Management

   * Declartive approch
      ... examples:
            .. Ansible
            .. Chef
            .. Puppet
            .. Powershell DSC

   Declartive: i want a file 
   Procedural: use some thing  i need some file
  
  ...Declarative Aproches:

  ###  Ansible: Configuration Management
  ###  Teraform: Infra provisioning
  ###  Kubernetes: Container Orchastration

* Configuration management is all about declarative deployement of application which ensures

* Idempotence: Run this once or 'n' times you will have same result
* Desired state: we express configuration to archive a desired state
* reusable state:  what we write the scrpit it can be reusable any time 

### Day 3 :  Configuration management

  ... configuration management server
  ... nodes
  ... push based 

* 1. Push Based Configuration Management : hi u need to execute cm
* 2. Pull Based Configuraion Management: node hi do i have any work to do

* Direction of communication

 push=>> CM Server to Node

  * list of nodes (inventory)
  * credentials to login in to nodes

  * Examples :  Ansible && Salt Stack


 pull=>> Node to CM server
  
  *  agent : needs to be install necssary configs
   
  *  examples: Cheff & Puppet

Infraprovisiong : Teraform >>

Teraform call ansible 


ansible simple installation //ansible controll node /// 
it automation tool network also
call ansible from ci cd tools



### Archtecure

 >> Ansible Controle Node  install ansible >>> Node1 to Node 2

   * ansible suppose has to be a linux machine controll node
   * Node can be windows machine also
   * Ansible devoped in python language
   * Ansible expects nodes need to be install python

Desired state of the ansible called as Playbook
 
 Inventory : inventry consist of which nodes should server speek with
    * 1. use name pw details
  ansible tries to login to nodes 
  ssh into it 
  desired state is converted to commands ( playbooks)
  executes
node should have python installaation

inventory:
playbook:

how to install ansible 
how to write playbooks

Ansible controll node can be execute desired state 2 ways

\\\\ adhoc commands
\\\\ playbooks

playbooks are YAMl files.

### Day4 Team work on muiltiple Servers

* Admin group : Users will be added to be admin group

* Service account : Account is given to admin which is manage and all of the users login and do the changes over user system



* user name and password is not a good idea

### Ansible installation 

steps

```
* Create 2 instances  and name as the--- Ansible controll  & node1
* check python :python3 --version  in both machines
* Adding user in both instaces: sudo adduser shiva--username in both machines
* Giving sudoers permissin to users: sudo visudo in both machines
* Chanange pw authentication as Yes : sudo vi /etc/ssh/sshd_cofig in both machines
* Restart sshd config :  sudo systemctl sshd restart in both machines (or ) sudo service sshd restart
* switch user : su shiva in both machines
* ssh-kegen in ansible controll node: ssh-keygen
* connect node1 using private ip node:  ssh-copy-id shiva@10.160.0.4
* check both machines sudo permissions : sudo apt update
* connect directly to user using ssh ip address and name :ssh 10.160.0.4 & ssh node1
* install ansible: in controll node
* sudo apt update
* sudo apt install software-properties-common -y
* sudo add-apt-repository --yes --update ppa:ansible/ansible
* sudo apt install ansible -y
* check ansible version
* shiva@asnible:~$ ansible --version
* ansible 2.10.8
```

``` 
vi hosts : edit ip adress and names 
check connectivity command: 'ansible -m ping -i hosts all'
ssh-copy-id shiva@localhost


Result--

shiva@asnible:~$ ansible -m ping -i hosts all
10.160.0.4 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"

}

```
Desired state :

adhoc commands:
playbooks:

sample playbook:
```yaml
---
- name: hello ansible
  hosts: all
  become: yes
  tasks:
    - name: update package and install tree
      apt:
        name: tree
        state: present
        update_cache: yes
```
check tree in node 1-- 

Activity1:
----------
Installation apache server

* list down manual steps
  - for eachstep find the module express steps
  - 
  ```
  sudo apt update
  sudp apt install apache2 -y
  ```
  verify the installation "http://public-ip"

* then write the ansible playbook

- name : It means about the inforamtion doing work
  host: Ip connectvity selection
  become: what ever using in ansible using sudo permissions
  tasks: what u want too do

```yaml
---
- name: install apache server
  hosts: all
  become: yes
  tasks:
    - name: install apache server
      ansible.builtin.apt:
        name: apache2
        update_cache : yes
        state: present
---      
```
```
 vi playbooks/apach2.yaml
 ansible-playbook -i inventory/hosts playbooks/apach2.yaml \
```
* observed the changes 

### install Lamp server 
-----------------------
--- Manual steps
```
sudo apt update\
sudo apt install apache2 -y
sudo apt install php libapache2-mod-php php-mysql -y
sudo -i
ls
sudo systemctl restart apache2
history 
```
 ### play book for lamp server...
 -------------------------------

 skip mysql server:

```yaml
---
- name: install lamp server on ubuntu
  hosts: all
  become: true
  tasks:
    - name: install apache2 on ubuntu
      ansible.builtin.apt:
        name: apache2
        update_cache: true
        state: absent
    - name: install php packages
      ansible.builtin.apt:
        name:
          - php
          - libapache2-mod-php
          - php-mysql
        update_cache: true
        state: present
    - name: copy the info.php page
      ansible.builtin.copy:
        src: info.php
        dest: /var/www/html/info.php
    - name: restart apache2
      ansible.builtin.systemd:
        name: apache2
        state: restarted
```
### Manual steps for Lamp in redhat:
-----------------------------------

```
sudo yum install httpd -y
sudo systemctl enable httpd
sudo yum install php -y
sudo -i 
sudo systemctl start httpd
echo '<?php phpinfo(); ?>' > /var/www/html/info.php
exit
sudo systemctl restart httpd
```
 >>> install ansible in redhat
 ------------------------------
```
*  Create 2 instances  and name as the--- Ansible controll  & node1
*  check python :python3 --version  in both machines
*  Adding user in both instaces: sudo adduser shiva1--username in both machines 
*  sudo useradd shiva1
*  cat /etc/passwd
*  sudo passwd shiva1
*  Giving sudoers permissin to users: sudo visudo in both machines
*  Chanange pw authentication as Yes : sudo vi /etc/ssh/sshd_cofig in both machines
*  Restart sshd config : sudo systemctl sshd restart in both machines (or ) sudo service sshd restart
*  switch user : su shiva1 in both machines
*  ssh-kegen in ansible controll node: ssh-keygen
*  connect node1 using private ip node:  ssh-copy-id shiva@10.160.0.4
*  curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
*  python3 get-pip.py --user
*  python3 -m pip -V
*  python3 -m pip install --user ansible
*  ansible --version
```
--- playbook for lamp in redhat
-------------------------------

```yaml
---
- name: install lamp server on redhat
  hosts: all
  become: true
  tasks:
    - name: install httpd on server
      ansible.builtin.yum:
        name: httpd
        update_cache: true
        state: present
    - name: start httpd service and enable
      ansible.builtin.systemd:
        name: httpd
        enabled: true
        state: started
    - name: install php server
      ansible.builtin.yum:
        name: php
        update_cache: true
        state: present
    - name: start httpd service and enable
      ansible.builtin.systemd:
        name: httpd
        state: started 
    - name: create info.php page
      ansible.builtin.copy:
        content: '<?php phpinfo(); ?>' 
        dest: /var/www/html/info.php
    - name: restart httpd service 
      ansible.builtin.systemd:
        name: httpd
        state: restarted 
```
' ansible-playbook -i inventory/hosts 
--syntax-check playbooks/lamp.yaml '
### Handlers:

--- notify:
     before execution of handlers
    Handelers are used overwrite everytime restart excecution with out down time

```yaml
---
  - name: install lamp server on ubuntu
    hosts: all
    become: yes
    tasks:
      - name: install apache server
        ansible.builtin.apt:
          name: apache2
          update_cache: true
          state: present
      - name: install php packages
        ansible.builtin.apt:
          name:
            - php
            - libapache2-mod-php
            - php-mysql
          update_cache: true
          state: present
        notify:
          - restart apache2
      - name: create info.php page
        ansible.builtin.copy:
          content: '<?php phpinfo(); ?>'
          dest: /var/www/html/info.php
        notify:
          - restart apache2
    handlers:
      - name: restart apache service
        ansible.builtin.systemd:
          name: apache2

---
```
Restart apache several time issue cleared

### INVERNTORY ...
------------------
* create inventory & ini files formate 
```
ansible-playbook -i inventory/hosts.yaml playbookscomb/lamp --list-host
ansible-playbook -i inventory/hosts.yaml playbookscomb/lamp
```
* INI formate &  & inverntory.yaml files formate

```
ansible-playbook -i inventory/hosts playbookscomb/lamp --list-host
ansible-playbook -i inventory/hosts playbookscomb/lamp
```
### playbooks to install the following on ubuntu 22.04 Manual
>>>	java 17 >>>
```
* sudo apt update
* sudo apt-get upgrade
* sudo apt install openjdk-17-jdk openjdk-17-jre
* java -version
* history
```
*   Play book for Java17
-------------------------
```yaml
---
  - name: java installation on ubuntu 22.04
    hosts: all
    become: true
    tasks:
      - name: install openjdk-17-jdk openjdk-17-jre packages
        ansible.builtin.apt:
          name: 
            - openjdk-17-jdk
            - openjdk-17-jre
          update_cache: yes
          state: present  
```
#### FACTS 
----------
```
ansible -m 'setup' -i '172.31.13.7, ' all 

ram
```