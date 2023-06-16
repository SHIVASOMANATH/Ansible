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

* Admin group : Users combination group

* Service account : 

