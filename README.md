## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(Images/RedTeam_network_diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the configuration file may be used to install only certain pieces of it, such as Filebeat.

  - filebeat-config.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly secure, in addition to restricting inbound access to the network.

  - Load balancers distribute web traffice across different servers helps mitigate DoS attacker. Increase capapcity and reliablity of applications.
  
  - Jump box prevents all virtural machines expose to the public. Restrict the communication with the Jump box, helps us to open only one port instead of several ports.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems and system metrics.

  - Filebeat collects log files from on remote machines. 
  
  - Metricbeat periodically collect metrics from the operating system and from services running on the server, such as uptime and CPU usage.

The configuration details of each machine may be found below.

| Name                 | Function          | IP Address | Operating System |
|----------------------|-------------------|------------|------------------|
| Jump Box Provisioner | Gateway           | 10.0.0.4   | Linux            |
| Web-1                | Web Server        | 10.0.0.5   | Linux            |
| Web-2                | Web Server        | 10.0.0.6   | Linux            |
| ELK-Server           | Monitor, Analytic | 10.1.0.4   | Linux            |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 14.200.xx.xx

Machines within the network can only be accessed by Jump Box.
Web servers 10.0.0.5 and 10.0.0.6 are allowed sending traffic to ELK server.

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses |
|----------------------|---------------------|----------------------|
| Jump Box Provisioner | Yes                 | 14.200.xx.xx         |
| ELK-Server           | Yes                 | 14.200.xx.xx         |
| Web-1                | No                  | 10.0.0.4             |
| Web-2                | No                  | 10.0.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible helps reduce repetitive tasks, and model complex workflows.

The playbook implements the following tasks:
- Install Docker engine: docker.io
- Install Python software
- Install Python client for Docker that used to control the state of Docker containers
- Download the Docker container image
- Configures the container port mappings


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
(Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1: 10.0.0.5
- Web-2: 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat collect log files on remote machines and ship to Logstash and Elasticsearch. Such as specify to collect access logs from Apache, which we can keep track of all requests processed by the Apache server.

- Metricbeat collecting machine metrics and service metrics for monitoring their performace, such as CPU usage and memory load to determine whether malicious service is running.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
--- Filebeat
- Copy the playbook YAML file to control node's /etc/ansible/.
- Update the hosts file to include which virtural machines to run each playbook on
- Run the playbook, and navigate to http://[your.VM.IP]:5601/app/kibana to check that the installation worked as expected.

Specific commands

- To download Playbook from Git
    $ curl https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > filebeat-config.yml

- To Update the hosts file
     - $ nano /ect/ansible/hosts
	 - [webservers]
         - 10.0.0.5
         - 10.0.0.6
         - [elk]
         - 10.1.0.4

- Run the playbook  
    - $ ansible-playbook install-elk.yml 
    - $ ansible-playbook filebeat-playbook.yml
    - $ ansible-playbook metricbeat-playbook.yml	
