# ELK-Stack-Project
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Link to Network Diagram](https://github.com/waseem2021/ELK-Stack-Project/blob/main/Diagrams/ELKStackNetworkDiagram.JPG)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml file may be used to install only certain pieces of it, such as Filebeat.

[Link to Filebeat playbook](https://github.com/waseem2021/ELK-Stack-Project/blob/main/Ansible/Filebeat-yml.txt)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant, in addition to restricting exposure to the network.
The off-loading function of a load balancer defends an organization against distributed denial-of-service (DDoS) attacks. It does this by shifting attack traffic from the corporate server to a public cloud provider. A jump server is a hardened and monitored device that spans two dissimilar security zones and provides a controlled means of access between them.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the filenames and system metrics.

Filebeat monitors the logs in the specified locations. It detects changes to the filesystem. It collects logs and forwards them to Elasticsearch or Logstash for indexing
Metricbeat detects changes in system metrics such as CPU usage. We use it to detect SSH login attempts


The configuration details of each machine may be found below.

| Name     | Function   | IP Address | Operating System |
|----------|------------|------------|------------------|
| Jump Box | Gateway    | 10.0.0.7   | Linux            |
| Web 1    | Server     | 10.0.0.12  | Linux            |
| Web 2    | Server     | 10.0.0.13  | Linux            |
| ELK      | Monitoring | 10.1.0.4   | Linux            |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the  Jumpbox Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
68.59.39.173


Machines within the network can only be accessed by peer servers.
Jump-Box-Provisioner (10.0.0.7)

A summary of the access policies in place can be found in the table below.


| Name     | Publicly Accessible | Allowed IP Address |
|----------|---------------------|--------------------|
| Jump Box | Yes                 | 68.59.39.173       |
| Web 1    | No                  | 10.0.0.1-254       |
| Web 2    | No                  | 10.0.0.1-254       |
| ELK      | No                  | 10.0.0.1-254       |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because
using Ansible allowed me to quickly install, update, and add the web servers to the network using the same playbooks

The playbook implements the following tasks:

Install docker on all network machines so they will be able to recieve and install containers
Ansible is installed on the Jump Box VM to distribute containers to other VMs on the network
Ansible playbooks are used to install the ELK stack container on the ELK server and a 'Beats' containers on the Web servers


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[Link to sudo_docker_ps screenshot](https://github.com/waseem2021/ELK-Stack-Project/blob/main/Diagrams/sudo_docker_ps.JPG)

Target Machines & Beats

ELK server is configured to monitor following machines

| Name  | IP Address         |
|-------|--------------------|
| Web 1 | 10.0.0.12          |
| Web 2 | 10.0.0.13          |

We have installed the following Beats on these machines:

Filebeat

These Beats allow us to collect the following information from each machine:

Filebeat - Filebeat detects changes to the filesystem. We are using this to monitor our Web Log data.

ansible-playbook elk-playbook.yml

ansible-playbook filebeat-playbook.yml

Using the Playbook In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:

Copy the .yml files to /etc/ansible directory.
Update the hosts file to be as follows. This will assign the VM servers to their server groups for the Ansible Playbooks.
   [webservers]
    10.0.0.12
    10.0.0.13
   [elkservers]
    10.1.0.4

Run the playbooks and install ELK, Filebeat run the following:
cd /etc/ansible
ansible-playbook elk-playbook.yml
ansible-playbook filebeat-playbook.yml

Kibana activities:

Mass SSH Logins
1. Ran SSH command in my jumpbox - ssh RedTeamAdmin@10.0.0.12
2. An error was also logged and sent to Kibana.
3. Ran the failed SSH command in a loop - for i in {1..1000}; do ssh sysadmin@10.0.0.12; done
4. Located the failed login attempts in Kibana
5. Repeated the above steps for my second VM - 10.0.0.13

View Kibana log after Linux Stress 
1. From Jump-Box, started up Ansible container and attached to it.
2. SSH from Ansible container to one of my WebVM's.
3. Run sudo apt install stress to install the stress program.
4. Run sudo stress --cpu 1 and allow stress to run for a few minutes.
5. Viewed the Metrics page for that VM in Kibana and there was CPU usage increase.
