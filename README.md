# Automated ELK Stack Deployment Project1

*The files in this repository were used to configure the network depicted below.

[Project 1 Red-Team Network Diagram](https://github.com/adriandefry/cybersecurity/blob/main/Diagrams/Red-Team%20Network%20Diagram.png)

*These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Project 1 Red-Team Network Diagram file may be used to install only certain pieces of it, such as Filebeat.

   [filebeat-playbook.yml](https://github.com/adriandefry/cybersecurity/blob/main/Ansible/filebeat_playbook.yml.txt) 
   
   [filebeat-configuration.yml](https://github.com/adriandefry/cybersecurity/blob/main/Linux/filebeat_configuration.yml.txt) 

*This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

*The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

- Load balancing ensures that the application will be highly functional, in addition to restricting high-traffic to the network.

- What aspect of security do load balancers protect? 

	- It helps prevent overloading servers as well as optimizes productivity and maximizes uptime. 
	- It also adds resiliency by rerouting live traffic from one server to another causing it to eliminate single points of failure from attacks such as DDoS attack.

- What is the advantage of a jump box?

	-Jump-box are highly secured computers that are never used for non-admin tasks. 
	-Throughout the years, jump-box has improved into an even more comprehensive/lock-down secure admin workstation to decrease the chances of hackers/malware infection. 


*Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the ___network and system logs.

- What does Filebeat watch for?
	- It monitors the log files/locations that you specify and forwards them to Elasticsearch/Logstash for indexing.
 
- What does Metricbeat record?
	- It records metrics/statistics data and transports them to the output that you specifics thru Elasticsearch/Logstash.


*The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name                 | Function | IP Address                               | Operating System     |
|----------------------|----------|------------------------------------------|----------------------|
| Jump-Box-Provisioner | Gateway  | 10.0.0.5(Private)//23.96.123.102(Public)|     Linux            |
| ELK-VM               | Server   | 10.1.0.4(Private)//13.66.196.172(Public)   |  Linux            |
| WEB-1              | Server   | 10.0.0.7(Private)                       |     Linux            |
| WEB-2              | Server   | 10.0.0.6(Private)                       |     Linux            |
| WEB-3              | Server   | 10.0.0.8(Private)                       |     Linux            |

### Access Policies

*The machines on the internal network are not exposed to the public Internet. 

- Only the jump-Box-Provisioner machine can accept connections from the Internet.
- Access to this machine is only allowed from the following IP addresses:
	- 71.59.34.72 (LocalHost IP address)

*Machines within the network can only be accessed by Jump-Box-Provisioner

- Which machine did you allow to access your ELK VM?
	- Jump-Box-Provisioner  

- What was its IP address?
	- 10.0.0.5 (Private) 

- A summary of the access policies in place can be found in the table below.

| Name                  | Publicly Accessible | Allowed IP Addresses |
|-----------------------|---------------------|----------------------|
| Jump-Box-Provisioner  |       Yes           | 71.59.34.72          |
| ELK-VM*               |       No            | 10.0.0.4             |
| WEB-1*    	          |       No            | 10.0.0.4             |
| WEB-2*       	        |       No            | 10.0.0.4             |
| WEB-3*       	        |       No            | 10.0.0.4             |

* --All these VMs can only be accessed form the Jump-Box-Provisioner--

### Elk Configuration

*Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- What is the main advantage of automating configuration with Ansible?

	- One main advantage would be YAML Playbooks. It is the best alternative for configuration management/automation.
	- It is also able to automate complex multi-tier IT application environemtns.     
                               
*The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._

	- First I, SSH into the Jump-Box-Provisioner (ssh sysadmin@23.96.123.102)
	- Start/Attached to the ansible docker (sudo docker start -i sharp_gauss)/(sudo docker attach sharp_gauss)
	- Went to /etc/ansible/roles directory and created the ELK playbook (Elk_Playbook.yml)
	- Ran the Elk_Playbook.yml in that same directory (ansible-playbook Elk_Playbook.yml)
	- Lastly, I SSH into the ELK-VM to verify the server is up and running.

*The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[ELK-VM Docker PS](https://github.com/adriandefry/cybersecurity/blob/main/Images/ELK-VM%20Docker%20PS.png)

### Target Machines & Beats
*This ELK server is configured to monitor the following machines:

- List the IP addresses of the machines you are monitoring

	- WEB-1 (10.0.0.7)
	- WEB-2 (10.0.0.6)
	- WEB-3 (10.0.0.8)
	

- We have installed the following Beats on these machines:

	- [Filebeat Module Status Screenshot](https://github.com/adriandefry/cybersecurity/blob/main/Images/filebeats_verify.png)
	- [Metricbeat Module Status Screenshot](https://github.com/adriandefry/cybersecurity/blob/main/Images/metricbeat_verify.png)

*These Beats allow us to collect the following information from each machine:
- In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
	
	- Filebeat is used to collect log files from specific files on remote machines.
	- Examples of Filebeats can be files that are generated by Apache, Microsoft Azure tools, the Nginx web server, and MySQl databases.	

	- Metricbeat collects machine metrics.
	- It is simply a measurement to tell analysts how healthy it is.
	- Examples of Metricbeat can be CPU usage/Uptime 

### Using the Playbook
*In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

---Filebeat---

- Copy the filebeat-configuration.yml file to /etc/ansible/roles/files.
- Update the filebeat-configuration.yml file to include the ELK private IP in lines 1106 and 1806.
- Run the playbook, and navigate to http://13.66.196.172:5601/ (ELK-VM public IP) to check that the installation worked as expected.

---Metricbeat---

- Copy the metricbeat-configuration.yml file to /etc/ansible/roles/files.
- Update the metricbeat-configuration.yml file to include the ELK private IP in lines 62 and 96.
- Run the playbook, and navigate to http://13.66.196.172:5601/ (ELK-VM public IP) to check that the installation worked as expected.

_Answer the following questions to fill in the blanks:_

- _Which file is the playbook? filebeat-playbook.yml
- _Where do you copy it? /etc/ansible/roles
- _Which file do you update to make Ansible run the playbook on a specific machine? /etc/ansible/hosts file (IP of the Virtual Machines). 
- _How do I specify which machine to install the ELK server on versus which to install Filebeat on? I have to specify two separate groups in the etc/ansible/hosts file. One of the groups will be webservers which has the IPs of the VMs that I will install Filebeat to. The other group is named elkservers which will have the IP of the VM I will install ELK to.
- _Which URL do you navigate to in order to check that the ELK server is running? http://13.66.196.172:5601/ 

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

      -------Filebeat---------

	- To create the filebeat-configuration.yml file: nano filebeat-configuration.yml. For this, I used the filebeat configuration file template.
	
	- To create the playbook: nano filebeat-playbook.yml
	
      ---
 	 - name: installing and launching filebeat
    	   hosts: webservers
           become: true
           tasks:

    	   - name: download filebeat deb
      	     command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.7.1-amd64.deb

    	   - name: install filebeat deb
      	     command: dpkg -i filebeat-7.7.1-amd64.deb

    	   - name: drop in filebeat.yml
      	     copy:
       	       src: ./files/filebeat-configuration.yml
       	       dest: /etc/filebeat/filebeat.yml

    	   - name: enable and configure system module
      	     command: filebeat modules enable system

    	   - name: setup filebeat
      	     command: filebeat setup

    	   - name: start filebeat service
      	    command: service filebeat start
	---
	-To run the playbook: ansible-playbook filebeat-playbook.yml
	
	* In order to run the playbook, you have to be in the directory the playbook is at, or give the path to it (ansible-playbook /etc/ansible/roles/filebeat-playbook.yml
	
	
	-------Metricbeat-------
	
	- To create the metricbeat-configuration.yml file: nano metricbeat-configuration.yml. For this, I used the metricbeat configuration file template.
	
	- To create the playbool: nano metricbeat-playbook.yml
	
	---
	  - name: installing and lunching metricbeat
	    hosts: webservers
	    become: true
	    tasks:
	    
	  - name: download metricbeat deb
	    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.7.1-amd64.deb
	    
	  - name: install metricbeat deb
	    command: sudo dpkg -i metricbeat-7.7.1-amd64.deb
	    
	  - name: drop in metricbeat.yml
	    copy:
	      src: /etc/ansible/roles/files/metricbeat-configuration.yml
	      dest: /etc/metricbeat/metricbeat.yml
	      
	   - name: enable and configure system module
	     command: metricbeat modules enable system
	     
	   - name: setup metricbeat
	     command: metricbeat setup
	     
	   - name: start metricbeat service
	     command: service metricbeat start
	     
	   ---
	   
	   - To run the playbook: ansible-playbook metricbeat-playbook.yml
	   
	   * To order to run the playbook, you have to be in the directory the playbook is at, or give the path to it (ansible-playbook /etc/ansible/roles/metricbeat-playbook.yml
	    
