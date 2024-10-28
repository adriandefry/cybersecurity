# 📊 Automated ELK Stack Deployment Project

*The files in this repository were used to configure the network depicted below:*

![Project 1 Red-Team Network Diagram](https://github.com/adriandefry/cybersecurity/blob/main/Diagrams/Red-Team%20Network%20Diagram.png)

*These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above, or select portions of the Project 1 Red-Team Network Diagram file to install specific components, such as Filebeat:*

- [filebeat-playbook.yml](https://github.com/adriandefry/cybersecurity/blob/main/Ansible/filebeat_playbook.yml.txt)  
- [filebeat-configuration.yml](https://github.com/adriandefry/cybersecurity/blob/main/Linux/filebeat_configuration.yml.txt)  

---

### 📋 Document Overview

*This document contains the following details:*

- **Description of the Topology**
- **Access Policies**
- **ELK Configuration**
  - Beats in Use
  - Machines Being Monitored
- **How to Use the Ansible Build**

---

### 🖥️ Description of the Topology

*The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.*

- **Load Balancing**: Ensures high functionality of the application while restricting high traffic.
- **Security Aspects of Load Balancers**:
  - Prevents server overload and optimizes productivity.
  - Adds resiliency by rerouting live traffic to eliminate single points of failure from attacks such as DDoS.

- **Advantages of a Jump Box**:
  - Jump boxes are highly secured computers, dedicated solely to admin tasks.
  - They minimize the chances of malware infection by maintaining a lockdown security environment.

*Integrating an ELK server allows users to easily monitor vulnerable VMs for changes in network and system logs:*

- **Filebeat**: Monitors specified log files and forwards them to Elasticsearch/Logstash for indexing.
- **Metricbeat**: Records metrics/statistics data and transports them to the specified output via Elasticsearch/Logstash.

---

### 🖥️ Configuration Details

*The configuration details of each machine are listed below. Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table.*

| 🖥️ Name                  | 📦 Function | 🌐 IP Address                               | 🐧 Operating System     |
|----------------------|----------|------------------------------------------|----------------------|
| Jump-Box-Provisioner | Gateway  | 10.0.0.5 (Private) / 23.96.123.102 (Public) | Linux            |
| ELK-VM               | Server   | 10.1.0.4 (Private) / 13.66.196.172 (Public) | Linux            |
| WEB-1                | Server   | 10.0.0.7 (Private)                       | Linux            |
| WEB-2                | Server   | 10.0.0.6 (Private)                       | Linux            |
| WEB-3                | Server   | 10.0.0.8 (Private)                       | Linux            |

---

### 🔑 Access Policies

*The machines on the internal network are not exposed to the public Internet:*

- Only the **Jump-Box-Provisioner** can accept connections from the Internet.
- Access is only allowed from the following IP addresses:
  - **71.59.34.72** (LocalHost IP address)

*Internal machines can only be accessed via Jump-Box-Provisioner:*

- **Allowed Access to ELK VM**: 
  - **Machine**: Jump-Box-Provisioner  
  - **IP Address**: 10.0.0.5 (Private) 

*Summary of the access policies:*

| 🖥️ Name                  | 🌐 Publicly Accessible | 📜 Allowed IP Addresses |
|-----------------------|---------------------|----------------------|
| Jump-Box-Provisioner  | Yes                 | 71.59.34.72          |
| ELK-VM                | No                  | 10.0.0.4             |
| WEB-1                 | No                  | 10.0.0.4             |
| WEB-2                 | No                  | 10.0.0.4             |
| WEB-3                 | No                  | 10.0.0.4             |

*Note: All these VMs can only be accessed from the Jump-Box-Provisioner.*

---

### ⚙️ ELK Configuration

*Ansible was used to automate the configuration of the ELK machine. No manual configuration was performed, which offers the following advantages:*

- **Automation Benefits**:
  - YAML Playbooks streamline configuration management and automation.
  - Capable of automating complex multi-tier IT application environments.

*The playbook implements the following tasks:*

- First, SSH into the Jump-Box-Provisioner (ssh sysadmin@23.96.123.102).
- Start/Attach to the Ansible Docker (sudo docker start -i sharp_gauss)/(sudo docker attach sharp_gauss).
- Navigate to `/etc/ansible/roles` directory and create the ELK playbook (`Elk_Playbook.yml`).
- Execute the playbook (ansible-playbook Elk_Playbook.yml).
- SSH into the ELK-VM to verify the server status.

*The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance:*

![ELK-VM Docker PS](https://github.com/adriandefry/cybersecurity/blob/main/Images/ELK-VM%20Docker%20PS.png)

---

### 🖥️ Target Machines & Beats

*This ELK server is configured to monitor the following machines:*

- **IP Addresses of Monitored Machines**:
  - WEB-1: 10.0.0.7
  - WEB-2: 10.0.0.6
  - WEB-3: 10.0.0.8

*Installed Beats on these machines:*

- ![Filebeat Module Status](https://github.com/adriandefry/cybersecurity/blob/main/Images/filebeats_verify.png)
- ![Metricbeat Module Status](https://github.com/adriandefry/cybersecurity/blob/main/Images/metricbeat_verify.png)

*Beats collect the following information:*

- **Filebeat**: Collects log files from specified locations on remote machines (e.g., logs generated by Apache or MySQL).
- **Metricbeat**: Collects machine metrics such as CPU usage and uptime.

---

### 🛠️ Using the Playbook

*To use the playbook, you need a pre-configured Ansible control node. SSH into the control node and follow these steps:*

#### --- Filebeat ---

- Copy the `filebeat-configuration.yml` file to `/etc/ansible/roles/files`.
- Update the `filebeat-configuration.yml` file to include the ELK private IP in lines 1106 and 1806.
- Run the playbook and navigate to [http://13.66.196.172:5601/](http://13.66.196.172:5601/) (ELK-VM public IP) to verify the installation.

#### --- Metricbeat ---

- Copy the `metricbeat-configuration.yml` file to `/etc/ansible/roles/files`.
- Update the `metricbeat-configuration.yml` file to include the ELK private IP in lines 62 and 96.
- Run the playbook and navigate to [http://13.66.196.172:5601/](http://13.66.196.172:5601/) (ELK-VM public IP) to verify the installation.

---

### 🔍 Questions & Answers

- **Which file is the playbook?** `filebeat-playbook.yml`
- **Where do you copy it?** `/etc/ansible/roles`
- **Which file do you update to run the playbook on a specific machine?** `/etc/ansible/hosts`
- **How do I specify the installation machine for ELK vs. Filebeat?** By defining separate groups in `/etc/ansible/hosts`: one for webservers (Filebeat) and another for elkservers (ELK).
- **What URL do I use to check if the ELK server is running?** [http://13.66.196.172:5601/](http://13.66.196.172:5601/)

---

### 🎉 Bonus: Commands

#### ------- Filebeat ---------

- To create the configuration file: 
 ```bash
  nano filebeat-configuration.yml
```
- To create the playbook:
 ```bash
nano filebeat-playbook.yml
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
```
- To run the playbook:
```bash
ansible-playbook filebeat-playbook.yml
```
------- Metricbeat ---------
- To create the configuration file:
```bash
nano metricbeat-configuration.yml
```
- To create the playbook:
```bash
nano metricbeat-playbook.yml
---
- name: installing and launching metricbeat
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
```
- To run the playbook:
```bash
ansible-playbook metricbeat-playbook.yml
```
