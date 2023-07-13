<h1>Hardening using Ansible</h1>


<h2>Description</h2>
This lab focuses on using Ansible to implement system hardening practices, which enhance the security and resilience of IT infrastructures. Ansible provides a powerful and automated approach to apply security configurations, enforce best practices, and mitigate common vulnerabilities across a fleet of systems.
<br />


<h2>Languages and Utilities Used</h2>

- <b>Ansible</b>

<h2>Environments Used </h2>

- <b>Windows 11</b>
- <b>Linux</b> 

<h2>Program walk-through:</h2>

First, we need to install the ansible and ansible-lint programs: <br/>
- pip3 install ansible==6.4.0 ansible-lint==6.8.1<br/>
 
<p align="center">
<img src="https://i.imgur.com/w9uodZr.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Let’s create the inventory or CMDB file for Ansible using the following command: <br/>
<br/>
 
<p align="center">
<img src="https://i.imgur.com/KS3Sy9w.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Next, we will have to ensure the SSH’s yes/no prompt is not shown while running the ansible commands, so we will be using ssh-keyscan to capture the key signatures beforehand: <br/>
- ssh-keyscan -t rsa prod-rcgsg0ei devsecops-box-rcgsg0ei >> ~/.ssh/known_hosts
<br/>
 
<p align="center">
<img src="https://i.imgur.com/TDq6lAV.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

We can use apt ansible module to install the ntp service on the production machine: <br/>
- ansible -i inventory.ini prod -m apt -a "name=ntp state=present"<br/>
 
<p align="center">
<img src="https://i.imgur.com/GlbpKxA.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Instead of restricting the commands to the prod machine, let’s find the bash version installed on all the machines in the inventory file: <br/>
- ansible -i inventory.ini all -m command -a "bash --version"<br/>
 
<p align="center">
<img src="https://i.imgur.com/7rrx8V9.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Use the Ansible command module to find shell module: <br/>
- ansible-doc -l | grep shell<br/>
 
<p align="center">
<img src="https://i.imgur.com/kx817sr.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Use inventory file (-i inventory.ini) and run the uptime command on the production machine using shell module: <br/>
- ansible -i inventory.ini prod -m shell -a "uptime"<br/>
 
<p align="center">
<img src="https://i.imgur.com/zjjegjV.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Let’s create a playbook to run against the production environment: <br/>
<br/>
 
<p align="center">
<img src="https://i.imgur.com/UDZM7ne.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Let’s run this playbook against the prod machine: <br/>
- ansible-playbook -i inventory.ini playbook.yml<br/>
 
<p align="center">
<img src="https://i.imgur.com/LE1wWPF.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Try running the above ansible command once again: <br/>
- ansible-playbook -i inventory.ini playbook.yml<br/>
 
<p align="center">
<img src="https://i.imgur.com/hGA7eBy.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Create a new directory /challenge and create a playbook.yml file inside the /challenge directory. You can use the above playbook.yml syntax as a starter for this task, but the /challenge/playbook.yml needs to use the secfigo.terraform role to install the Terraform utility: <br/>
- ansible-playbook -i inventory.ini playbook.yml<br/>
 
<p align="center">
<img src="https://i.imgur.com/OuhwpUd.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Install secfigo.terraform role using ansible-galaxy: <br/>
- ansible-galaxy install secfigo.terraform<br/>
 
<p align="center">
<img src="https://i.imgur.com/QcmQiw3.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Execute the /challenge/playbook.yml against the prod machine to install the Terraform utility. Optionally put this hardening job in the CI pipeline: <br/>
- ansible-playbook -i /challenge/inventory.ini /challenge/playbook.yml<br/>
 
<p align="center">
<img src="https://i.imgur.com/VSPbFvi.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Ansible galaxy helps you in storing open source Ansible roles.
Let’s explore the options it provides us: <br/>
- ansible-galaxy role --help<br/>
 
<p align="center">
<img src="https://i.imgur.com/OWc6MuL.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

We can search for desired roles using the search option: <br/>
- ansible-galaxy search terraform<br/>
 
<p align="center">
<img src="https://i.imgur.com/LVsHkAX.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

It’s a long list, but we will choose the secfigo.terraform role from the search results: <br/>
- ansible-galaxy install secfigo.terraform<br/>
 
<p align="center">
<img src="https://i.imgur.com/0S2gMC3.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Let’s create a directory and playbook to install the terraform on the production machine: <br/>
 
<p align="center">
<img src="https://i.imgur.com/hhvEEZi.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Let’s run this playbook against the prod machine to install the Terraform utility: <br/>
- ansible-playbook -i /challenge/inventory.ini /challenge/playbook.yml<br/>
 
<p align="center">
<img src="https://i.imgur.com/KflVRd7.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Create a new directory /hardening and create an ansible-hardening.yml file inside the /hardening directory. The /hardening/ansible-hardening.yml needs to use the dev-sec.os-hardening role to harden a target server: <br/>
 
<p align="center">
<img src="https://i.imgur.com/ZcIQqHX.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Install dev-sec.os-hardening role from ansible-galaxy: <br/>
- ansible-galaxy install dev-sec.os-hardening<br/>
 
<p align="center">
<img src="https://i.imgur.com/8YAa9PD.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />

Execute the /hardening/ansible-hardening.yml to harden the Ubuntu production machine. Optionally put this hardening job in the CI pipeline: <br/>
- ansible-playbook -i /hardening/inventory.ini /hardening/ansible-hardening.yml<br/>
 
<p align="center">
<img src="https://i.imgur.com/fdlg4eI.png" height="80%" width="80%" alt="Disk Sanitization Step"/>
</p>

<br />
<br />
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
