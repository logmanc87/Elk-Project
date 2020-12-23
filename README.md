## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

## Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting in-bound access to the network.

What aspect of security do load balancers protect? 

A load balancer intelligently distributes traffic from clients across multiple servers without the clients having to understand how many servers are in use or how they are configured. Because the load balancer sits between the clients and the servers it can enhance the user experience by providing additional security, performance, resilience and simplify scaling your website.

What is the advantage of a jump box?

A jump box is a secure computer that all admins first connect to before launching any administrative task or use as an origination point to connect to other servers or untrusted environments.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the jumpbox and system network

What does Filebeat watch for?

- Filebeat watches for changes to files on the machine.

What does Metricbeat record?

- Metricbeat collects metrics from the operating system and from services running on the server.

The configuration details of each machine may be found below.

| Name       | Function   | IP Address | Operating System |
|------------|------------|------------|------------------|
| Jump Box   | Gateway    | 10.0.0.7   | Linux            |
| Web-1      | Webserver  | 10.0.0.5   | Linux            |
| Web-2      | Webserver  | 10.0.0.6   | Linux            |
| ELK-Server | Monitoring | 10.1.0.4   | Linux            |

## Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- 71.135.133.0/24

Machines within the network can only be accessed by jump box provisioner.

Which machine did you allow to access your ELK VM? What was its IP address?

- My Jump Box 
  - Public IP: 52.249.181.84
  - Private IP: 10.0.0.7
  
A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Allowed IP Addresses |
|------------|---------------------|----------------------|
| Jump Box   | Yes                 | 71.135.133.0/24      |
| Web-1      | No                  | 10.0.0.7 & 10.1.0.4  |
| Web-2      | No                  | 10.0.0.7 & 10.1.0.4  |
| ELK-Server | No                  | 10.0.0.7 & 10.1.0.4  |


## Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous for the following reasons:

- Free: Ansible is an open-source tool.
- Very simple to set up and use: No special coding skills are necessary to use Ansible’s playbooks (more on playbooks later).
- Powerful: Ansible lets you model even highly complex IT workflows.
- Flexible: You can orchestrate the entire application environment no matter where it’s deployed. You can also customize it based on your needs.
- Agentless: You don’t need to install any other software or firewall ports on the client systems you want to automate. You also don’t have to set up a separate management structure.
- Efficient: Because you don’t need to install any extra software, there’s more room for application resources on your server.

The playbook implements the following tasks:

- Install docker.io
- Install pip3
- Install Docker python module
- Increase virtual memory
- Download and launch a docker

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](images/docker_ps_output.png)

## Target Machines & Beats

This ELK server is configured to monitor the following machines:

| Name   | IP Address |
|--------|------------|
| Web-1  | 10.0.0.5   |
| Web-2  | 10.0.0.6   |

We have installed the following Beats on these machines:

- Microbeats.

These Beats allow us to collect the following information from each machine:

- Filebeat: Collects data about the file system.
- Metricbeat: Collects machine metrics, such as uptime.

## Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the playbook file to Ansible Control Node. Playbooks for Filebeat and Metricbeat are also here: Link & Link

```
$ cd /etc/ansible
$ mkdir files
* Clone Repository + IaC Files
$ git clone https://github.com/logmanc87/Elk-Project.git
* Move Playbooks and hosts file Into `/etc/ansible`
$ cp /Elk-Project/ReadMe/Playbooks/*
```

- Update the hosts file to include webservers and elk.
- Edit hosts file to update and to make Ansible run the playbook on a specific machine, and specify which machine to install the ELK server on versus which to install Filebeat.
- Copy of the hosts file is also here: Link

```
$ cd /etc/ansible
$ nano hosts
# Ex 2: A collection of hosts belonging to the 'webservers' group
[webservers]
10.0.0.5 ansible_python_interpreter=/usr/bin/python3
10.0.0.6 ansible_python_interpreter=/usr/bin/python3

[elk]
10.1.0.4 ansible_python_interpreter=/usr/bin/python3
```

- Run the playbooks.

```
cd /etc/ansible
 $ ansible-playbook install_elk.yml elk
 $ ansible-playbook install_filebeat.yml webservers
 $ ansible-playbook install_metricbeat.yml webservers
 ```
 
 - Check that the ELK server is running: http://[Host IP]/app/kibana#/home
