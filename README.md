## Cloud-Based-Honeypot-Deployment-on-AWS

### A Brief Overview

We present the design and implementation of a honeypot leveraging a Raspberry Pi 5 running Raspberry Pi OS and Cowrie honeypot software. The Raspberry 
Pi serves as the foundational hardware platform, offering flexibility, affordability, and ease of deployment. Cowrie, a popular honeypot software, is chosen for its compatibility 
with Raspberry Pi and its ability to accurately mimic vulnerable network service such as SSH. 

The primary objective of this project is twofold: first, to create a realistic environment that emulates vulnerable network services to entice potential attackers, and second, to 
analyze the behaviors and tactics employed by attackers to gain insights into prevalent cyber threats. By capturing and studying attack patterns, the project aims to enhance 
network security measures and strengthen defenses against malicious actors.

To extend the capabilities of the honeypot setup, an Amazon EC2 instance is provisioned to serve as a remote monitoring and data collection point. This cloud-based 
infrastructure facilitates real-time monitoring and management of the honeypot environment while ensuring the integrity of the local network remains uncompromised.
This involves simulating various attack scenarios using Kali Linux as the attacker's machine, initiating SSH connections to the EC2 instance, and employing 
reconnaissance techniques to assess the honeypot's responsiveness and effectiveness. 
Data collected during these simulations, including connection attempts, commands executed by attackers, and their originating IP addresses, are logged by Cowrie for 
subsequent analysis.

Through comprehensive analysis of captured data, including log files and attack patterns, the project aims to identify common attack vectors, assess the severity and 
sophistication of detected attacks, and provide actionable insights for enhancing network security posture. The findings and observations derived from this analysis are 
documented extensively to contribute to the body of knowledge in cybersecurity research and inform future efforts in mitigating cyber threats.

### Hardware and Software Requirements

### Raspberry Pi 

![image](https://github.com/Vaibhav1730/Cloud-Based-Honeypot-Deployment-on-AWS/assets/116676361/ae623989-de75-4735-83c6-9fbbebcc4644)

![image](https://github.com/Vaibhav1730/Cloud-Based-Honeypot-Deployment-on-AWS/assets/116676361/e664f7a1-3f36-43b1-bd6d-b14a1ed9b822)
The Raspberry Pi 5 is a single-board computer renowned for its compact size, low cost, and impressive performance. Powered by a quad-core ARM Cortex-A72 processor 
running at up to 1.8GHz, coupled with 4GB of RAM, the Raspberry Pi 5 offers significant computing power for various applications. It features built-in wireless 
connectivity options such as Wi-Fi 6 and Bluetooth 5.0, along with Gigabit Ethernet for network connectivity. Additionally, it boasts a plethora of peripheral ports, including 
USB 3.0, HDMI, GPIO, and more, enabling seamless integration with a wide range of devices and sensors.

### Raspberry Pi Imager

![Screenshot 2024-05-04 212503](https://github.com/Vaibhav1730/Cloud-Based-Honeypot-Deployment-on-AWS/assets/116676361/84be815a-4af5-448f-92d2-2772e23123cb)
Raspberry Pi Imager is an official tool developed by the Raspberry Pi Foundation for easily installing operating systems onto SD cards or USB drives for use with Raspberry 
Pi computers. Here's a brief overview of its key features:

### Raspberry Pi OS

![Screenshot 2024-05-04 213110](https://github.com/Vaibhav1730/Cloud-Based-Honeypot-Deployment-on-AWS/assets/116676361/4ef81a44-7755-4305-bace-6e187da99af4)

![Screenshot 2024-05-23 162635](https://github.com/Vaibhav1730/Cloud-Based-Honeypot-Deployment-on-AWS/assets/116676361/8f25c4de-3189-4be1-aff6-3d991c93d8f9)

![Screenshot 2024-05-23 162712](https://github.com/Vaibhav1730/Cloud-Based-Honeypot-Deployment-on-AWS/assets/116676361/673b5208-d831-4b08-966b-a72a6c934811)

### Kali Linux

![image](https://github.com/Vaibhav1730/Cloud-Based-Honeypot-Deployment-on-AWS/assets/116676361/66e9e7fe-b035-4b25-aeeb-87a3d81447c1)

### Honeypot Firmware

![image](https://github.com/Vaibhav1730/Cloud-Based-Honeypot-Deployment-on-AWS/assets/116676361/25c24601-4cee-4d45-804f-c92f1163623a)

### AWS Instance 

![image](https://github.com/Vaibhav1730/Cloud-Based-Honeypot-Deployment-on-AWS/assets/116676361/1b91d305-3e6c-45ef-a993-058c38208dd6)

### Cowrie Honeypot

![image](https://github.com/Vaibhav1730/Cloud-Based-Honeypot-Deployment-on-AWS/assets/116676361/81f8a303-d3d2-4059-abd0-a615b45bca94)

### Block Diagram

![image](https://github.com/Vaibhav1730/Cloud-Based-Honeypot-Deployment-on-AWS/assets/116676361/14789b20-e54d-4d6c-9843-e6991db98495)

### Flowchart

![image](https://github.com/Vaibhav1730/Cloud-Based-Honeypot-Deployment-on-AWS/assets/116676361/2dc56221-f9dd-48ee-8cb8-0f1186835743)

### Steps to setup Cowrie Honeypot in Raspberry Pi 5 Environment

#### Change the Port You’ll Use to Administer the Server
Cowrie will be listening for SSH connections on port 22. You’ll want to configure the SSH service to listen on a different port for you to connect to and administer the server.
sudo vi /etc/ssh/sshd_config

Under # What ports, IPs and protocols we listen for, change the port number to 3393 or your preferred port number.

![image](https://github.com/Vaibhav1730/Cloud-Based-Honeypot-Deployment-on-AWS/assets/116676361/770860df-98c9-41e8-bcec-3fa23f6852e4)

Write your changes and quit vi.
Ctrl + C
:wq

Restart the SSH service.
service ssh restart

By running the command below, you can see that the server is now listening for connections on port 3393.
netstat -tan
Proto Recv-Q Send-Q Local Address Foreign Address State
tcp0 0 0.0.0.0:3393 0.0.0.0:* LISTEN

Install and Configure Cowrie
Download updated package lists.
sudo apt-get update

Install Cowrie’s dependencies.
sudo apt-get install python2.7 git virtualenv libmpfr-dev libssl-dev libmpc-dev libffi-dev build-essential libpython-dev python-pip

Add a new user named, cowrie.
sudo adduser — disabled-password cowrie

Switch to the new user, cowrie
sudo su — cowrie

Navigate to the home directory of user, cowrie, and clone the cowrie git repository.
git clone https://github.com/micheloosterhof/cowrie.git

















