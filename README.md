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

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/bcbae690-489c-476c-a385-f790ff510867)

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/18c2c3ff-56ea-4375-8e0e-c871fb784ae7)

The Raspberry Pi 5 is a single-board computer renowned for its compact size, low cost, and impressive performance. Powered by a quad-core ARM Cortex-A72 processor 
running at up to 1.8GHz, coupled with 4GB of RAM, the Raspberry Pi 5 offers significant computing power for various applications. It features built-in wireless 
connectivity options such as Wi-Fi 6 and Bluetooth 5.0, along with Gigabit Ethernet for network connectivity. Additionally, it boasts a plethora of peripheral ports, including 
USB 3.0, HDMI, GPIO, and more, enabling seamless integration with a wide range of devices and sensors.

### Raspberry Pi Imager

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/d7f65ac2-b868-4851-b3d0-424eddf98303)

Raspberry Pi Imager is an official tool developed by the Raspberry Pi Foundation for easily installing operating systems onto SD cards or USB drives for use with Raspberry 
Pi computers. Here's a brief overview of its key features:

### Raspberry Pi OS

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/c82db289-10e5-48d5-aa38-1614ba8950d8)

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/28babd00-d4e8-4d34-9726-455e7b2484d7)

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/5371b801-7baa-4be3-8c6c-f917b955e41e)

### Kali Linux

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/1dc74c6a-784f-4181-9450-95f11c48846e)

### Honeypot Firmware

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/9191f5e9-6a3f-4fed-920a-d65d92d6fc10)

### AWS Instance 

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/e46a9d88-8b4f-4df9-bdb1-fd042a92492a)

### Cowrie Honeypot

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/42606ee8-27c9-4beb-82bf-22c4d4a2dd81)

### Block Diagram

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/839d778a-8c74-438b-83bc-03f00a0a8d12)

### Flowchart

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/fce0bf4a-95e1-4022-b8f4-11bf3e5bb4fc)

## Steps to setup Cowrie Honeypot in Raspberry Pi 5 Environment

+ Change the Port You’ll Use to Administer the Server
Cowrie will be listening for SSH connections on port 22. You’ll want to configure the SSH service to listen on a different port for you to connect to and administer the server. <br />
**sudo vi /etc/ssh/sshd_config** 

+ Under # What ports, IPs and protocols we listen for, change the port number to 3393 or your preferred port number.

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/7fa0b47a-0c6e-419a-b3a0-19dec6b5fd28)

+ Write your changes and quit vi. <br />
**Ctrl + C <br />**
**:wq <br />**

+ Restart the SSH service. <br />
**service ssh restart** 

+ By running the command below, you can see that the server is now listening for connections on port 3393. <br />
**netstat -tan <br />**
Proto Recv-Q Send-Q Local Address Foreign Address State <br />
tcp0 0 0.0.0.0:3393 0.0.0.0:* LISTEN

+ Install and Configure Cowrie <br />
Download updated package lists. <br />
**sudo apt-get update**

+ Install Cowrie’s dependencies. <br />
**sudo apt-get install python2.7 git virtualenv libmpfr-dev libssl-dev libmpc-dev libffi-dev build-essential libpython-dev python-pip**

+ Add a new user named, cowrie. <br />
**sudo adduser — disabled-password cowrie**

+ Switch to the new user, cowrie <br />
**sudo su — cowrie**

+ Navigate to the home directory of user, cowrie, and clone the cowrie git repository. <br />
**git clone https://github.com/micheloosterhof/cowrie.git**

+ Create a new Python virtual environment for cowrie. <br />
**cd cowrie <br />**
**virtualenv cowrie-env**

+ Activate the virtual environment. <br />
**source cowrie-env/bin/activate**

+ The terminal will display (cowrie-env) before the username, cowrie.

+ Install pycrypto, Crypto and other requirements. <br />
**pip install pycrypto Crypto <br />**
(cowrie-env)$ **pip install -r requirements.txt**

+ Make a copy of the config file for your new cowrie instance. <br />
**cd /home/cowrie/cowrie/ <br />**
**cp cowrie.cfg.dist cowrie.cfg <br />**
**vi ./cowrie.cfg**

+ Set the hostname in the configuration file to a server name of your choice. E.g. fileserver4

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/ff3eb535-82e0-4485-80ec-4312805cf3e3)

+ Change the Port to listen for incoming SSH connections to port 22.

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/8199b0cb-ed79-4ed4-b029-21726e0f1d8f)

+ Write your changes and quit vi. <br />
**Ctrl + C <br />**
**:wq**

+ Enable authbind in cowrie’s start.sh file. <br />
**sudo vi /home/cowrie/cowrie/start.sh**

+ Change line 2 to read: <br />
**AUTHBIND_ENABLED=yes**

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/48229964-9f61-44c5-8416-ac57261a8da9)

**sudo apt-get install authbind <br />**
**sudo touch /etc/authbind/byport/22 <br />**
**sudo chown cowrie /etc/authbind/byport/22 <br />**
**sudo chmod 777 /etc/authbind/byport/22**

+ Execute the following commands to start Cowrie. <br />
**sudo su cowrie <br />**
**cd /home/cowrie/cowrie/ <br />**
**source cowrie-env/bin/activate <br />**
**./start.sh**

+ Verify cowrie is listening on port 22 by running the command below. <br />
**netstat -tan**

+ Execute the following command to stop Cowrie. <br />
**./stop.sh**

## Now set up an AWS EC2 instance with port-forwarding 

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/f7e426fd-c44e-4f67-92f5-1fbabd96c7a2)

+ Step 1: Create an ubuntu EC2 server on AWS <br />
EC2 instance specifications: <br />
**t2 nano is enough <br />**
**Create instance in the region which is closest to where your private server is so to keep the latency as low as possible.**

+ Once the EC2 instance is created: <br />
**Keep note of the EC2 public IP <br />**
**Noted XX.XX.XX.XX in this post <br />**
**Make sure to generate a .pem key to be able to connect to EC2 instance through ssh. Download it.**

+ Connect to the instance through ssh using the following commands: <br />
**export INSTANCE_IP=XX.XX.XX.XX <br />**
**ssh -i mykey.pem ubuntu@$INSTANCE_IP**

+ Step 2: Edit Security Group <br />
**Open port 8022 for connection from anywhere (0.0.0.0/0)**

![image](https://github.com/Vaibhav1730/AWS_Honeypot/assets/116676361/71c86136-2000-41b6-b660-db5a0166f171)

+ Step 3: Edit SSH config <br />
**Change in the EC2 instance GatewayPorts to yes in /etc/ssh/sshd_config file.**

+ Restart SSH service: <br />
**sudo /etc/init.d/ssh restart**

+ Step 4: Connect to private server <br />
**export PRIVATE_IP=192.168.68.120 <br />**
**ssh username@$PRIVATE_IP**

+ Step 5: Open a SSH connection between your private server and the EC2 instance <br />
**You need to have first a copy of the EC2 instance SSH key on your private server. <br />**
export INSTANCE_IP=XX.XX.XX.XX <br />**
**ssh -N -o ServerAliveInterval=20 -i mykey.pem -R 8022:127.0.0.1:22 ubuntu@$INSTANCE_IP &**

+ This command instructs AWS server to forward packages coming from its port 8022 to your private server port 22. <br />
**A ping is sent every 20 seconds to keep the connection alive & enables running the command in background.**

+ Step 6 (and last step): Connect to your private server from anywhere <br />
**To connect to tower, connect to server on port 8022: <br />**
**ssh -p 8022 username@$INSTANCE_IP**
