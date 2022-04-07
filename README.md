## Automated ELK Stack Deployment


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - Enter the playbook file. 
- name: Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:
    # Use command module
  - name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb
    # Use command module
  - name: Install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb
    # Use copy module
  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml
    # Use command module
  - name: Enable and Configure System Module
    command: filebeat modules enable system
    # Use command module
  - name: Setup filebeat
    command: filebeat setup
    # Use command module
  - name: Start filebeat service
    command: service filebeat start


This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the DVWA Vulnerable Web Application.

Load balancing ensures that the application will be highly resilient, in addition to restricting access to the network.
-	What aspect of security do load balancers protect? 
LD Balancer provides DDOS and redundancy protection.


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system _____.

- File Beat watches for changes in the system and log files.

- Metric Beat records metrics from each VM in the network.

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web 1    |  VM      | 10.0.0.5   | Linus            |
| Web 2    |  VM      | 10.0.0.6     | Linux          |
| Elk      |  VM      | 10.1.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpobox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 13.88.60.51

Machines within the network can only be accessed by the JumpBox and Elk VM’s

- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_
Web 1 and Web 2 both have access. Web1: 10.0.0.5 Web2: 10.0.0.6


A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 10.0.0.4             |
| Web 1    | No                  | 10.0.0.5             |
| Web 2    | No                  | 10.0.0.6             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

-What is the main advantage of automating configuration with Ansible?
The ability to easily scale up or down depending on what you need
The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. 
-	Installs docker.io
-	Installs Python3-pip
-	Installs Docker Mod 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

asek@Elk:~$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED      STATUS      PORTS                                                                              NAMES
d6dfc3f7a7f9   sebp/elk:761   "/usr/local/bin/star…"   9 days ago   Up 9 days   0.0.0.0:5044->5044/tcp, 0.0.0.0:5601->5601/tcp, 0.0.0.0:9200->9200/tcp, 9300/tcp   elk
asek@Elk:~$

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
-	10.0.0.5
      10.0.0.6

We have installed the following Beats on these machines:
-  filebeat-7.4.0 
   metricbeat-7.4.0

These Beats allow us to collect the following information from each machine:
- Filebeats allows us to collect agent host files that change for any reason. Metricbeats collects and logs host systems metrics such as operating system and other systems status.
