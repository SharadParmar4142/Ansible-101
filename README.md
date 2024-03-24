Ansible:THEORY

+Configuration Management
It is a way on how a devops engineer manages the configuration of infrastructure/server.

Why do we use ansible when we have other options too??
1 . We use ansible because it is used by most of the Devops engineers on a day to day basis
2 . Also it is easy to use compared to other languages
3 . Language: YAML
4 . It uses push mechanism
 --------------------------------
Ansible:PRACTICAL(Prerequisite for ansible is to first have password less authentication)

Steps to achive password less authentication are listed below::

To get the application of ansible we use 2 EC2 instance:

What are we doing?
We are trying to communicate with the secondary instance using the server as our main using password less authentication, so that we can configure the server remotely



-> The first one will be our main server and using that server we access the second one.

-> Use command ssh-keygen....To configure and do the as required by the system
This creates a private/public key within our system, where our private key is responsible for password less access to the secondary instance.
We can see the newly created files in the given address.

-> Copy the public address of the file created under id-rsa.pub and copy the content of that file.

-> Open a new terminal and log into that terminal with the target EC2, and use ssh-keygen.

-> Open the file "authorized-keys" under the given address and paste the content of the "id-rsa.pub"(main server) in that file and save the file.

-> Go back to the main server(Ubuntu) and use the command: ssh IP-address-private of the target EC2.
This will help you authenticate without using any password, and log into the target server.

 --------------------------------------------------------------------
 
Ansible playbooks(Here file are known as playbooks nothing much, just ansible file as ansible playbooks, some of them are prebuilt and some are created by the user)
Ansible adhoc commands (Playbooks that are commands which are already present in our system,and help us navigate if we want to use only a couple of command on our target server; we use playbook if we want to use more than a couple of commands on our target server)

-How to run ansible adhoc command and why are they used?
Adhoc command help us to execute commands on target server using only our main server, but before that we need to authenticate those server with password less authentication and store those ip address(private) in an inventory file in our main server.


Running ansible commands:
ansible -i inventory all -m "module-name" -a "arguments-to-be-passed"
    Eg:ansible -i inventory all -m "shell" -a "touch=devopsclass"

Explanation
Here the "inventory" file contains the list of target ip address; "all" is used so that the command applies to all the ip address present in it; "-m" is used as a prefix to let the system know that the next text is a module; 
"-a" is also a prefix -a stands for arguments and the next conatins all the arguments to be passed for that particular module.

We can use different module from internet just type ansible module and get the required modules.

 -------------------------------------------------------------------------------

-Targeting using multiple target servers::
To do the same we group the servers in our inventory of main server as:(inside inventory)
	[webservers]
	list of all the ip address in that servers

	[bdservers]
	list of all the ip address in that servers

-How to target a particular task for that particular group:
ansible -i inventory groupName -m "shell" -a "touch=devopsclass"
(ansible -i inventory bdservers -m "shell" -a "touch=devopsclass")

The format will be the same just we need to change the "bdservers" and write the group name.

 -------------------------------------------------------------------------------

-Using playbook command:
Playbooks are used for writing our own tasks instead of using some predefined commands, to use them playbook we write:
ansible-playbook -i inventory nameOFplaybook

The above command will execute the tasks written inside the playbook

Why we use ansible?
 We use ansible to configure our kubernates cluser.
 ---
Since we cannot write big tasks and run them, test them in one simple playbook, ansible has provided us with ansible roles which segregate our task into different sections where each part handles different roles, and help structure our playbook, namely::

vars = Same as defaults
tests = Performs the necessary tests for tasks
templates = Stores template(don't know it yet)
tasks = all the tasks are listed here
meta = Describes about the playbook and tells the users about the licence of the playbook given by the user
handlers  = It handles the exception(error if it pops up) that can be taken care of
files = If we want to share some files to another machine which we can't do using tasks for example we want to share index.html along with other machine we place index.html here
defaults = Storing variables
README.md = Tells about the file

Okay till now but how we will use them?
To use these we write the command: ansible-galaxy role init directoryName

Now, the above will create all the necessary directory for the same and its recommended to create an empty directory and then use the above command so that our work remains clean.

