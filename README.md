# aws-docker-challenge
Here is my Public IP, Public DNS, and Results:
* **Public IP:** 52.15.107.235
* **Public DNS**: ec2-52-15-107-235.us-east-2.compute.amazonaws.com
* **Results**: http://ec2-52-15-107-235.us-east-2.compute.amazonaws.com

By completing the below steps, you'll have:
 - Created an EC2 instance using CloudFormation
 - Create an EC2 intance using the AWS Console
 - Installed and configured an Ansible host
 - Installed Docker and deployed an nginx container
 - Created a webpage that can be view on the public internet via port 80

## Create an EC2 Instance for Docker and Nginx
1. Navigate to [Cloud Formation](https://aws.amazon.com/cloudformation/)​ and log in with your credentials.
2. Click the **Create Stack** button.  If a choice is provided, select "With new resources".
3. Under the "Specify Template" section, select the "Upload a template file" option, and then choose your file.  You'll want to use the file named [cf-ec2instance-ubuntu.yml](https://github.com/dustin-l-brown/aws-docker-challenge/blob/master/cf-ec2instance-ubuntu.yml).
4. Clicking next will bring you to the "Specify Stack Details" page.  Enter the following then hit **Next**:
    * **Stack Name:** WEX
    * **Image:** ami-0d5d9d301c853a04a
    * **InstanceType:** t2.micro
    * **KeyName:** *you'll need to provide your own*
    * **ServerName:** Docker-Nginx
5. On the "Configure stack options" page, press **Next** and on the following page, review the information then press **Create stack**.

## Setup and Configure Ansible
### (Optional) Create an EC2 server and install Ansible
If you already have Ansible configured somewhere, you can skip this step.
 1. Log into the [AWS Console](https://aws.amazon.com/console/).  Once in search for, or click on **EC2**.
 2. Press the **Launch Instance** button and search for the ami: `ami-0d5d9d301c853a04a`
 3. Press the **Review and Launch** button, followed by **Launch**
 4. When prompted, select your existing aws key pair and check the box stating that you have access to the selected private key.  At that point, press **Launch Instance**.
 5. Once your instance has come up, SSH into it and run the following commands:

		sudo apt-add-repository ppa:ansible/ansible
		sudo apt-get update -y
		sudo apt-get install ansible -y
  
### Configure our Ansible Inventory
 1. First we need to update our hosts file located in: `/etc/ansible/hosts`
 2. Open the file, and add the following lines to the end:
			
		[servers]
		server1 ansible_host=52.15.107.235

		[servers:vars]
		ansible_python_interpreter=/usr/bin/python3

 ### Configure SSH from our Ansible Host to Node 
 1. Create an SSH key by running `ssh-keygen`
 2. Answer yes when prompted, and specify a password if for additional security.
 3. When the file has been generated run the following: `cat ~/.ssh/id_rsa.pub`
 4. Copy the results to the clipboard
 5. Log (via ssh) into the EC2 instance we created with CloudFormation 
 6. Locate the following file, and append the information we copied from the Ansible server: `vi ~/.ssh/authorized_keys`
 7. Save the file and exit the server. 
 8.  If you've followed the above instructions you should now be able to run the following command:  `ansible all -m ping -u ubuntu`
 Providing the following sample output:
 > server1 | SUCCESS => {
 > "changed": false,
 > "ping": "pong"
 > }
## Run the Ansible Playbook
 1. From your Ansible Host, navigate to the playbook file named: [ans-provision.yml](https://github.com/dustin-l-brown/aws-docker-challenge/blob/master/ans-provision.yml)
 2. Run the following command: `ansible-playbook ans-provision.yml`

From here you should be able to launch your browser and access the nginx server via http:// followed by your Public DNS or Public IP.

**Congratulations!**
