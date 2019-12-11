# aws-docker-challenge


Read through the following and get back to us with any questions you might have, and a rough
time estimate on when you think you will be able to complete the challenge.

Using the ​AWS free tier​, create an account to accomplish the following tasks.
1. Using ​[Cloud Formation](https://aws.amazon.com/cloudformation/)​ create a new [​Ubuntu 18.04](https://ubuntu.com/)​ ​[EC2](https://aws.amazon.com/ec2/)​ instance and expose port 80 on
a public IP address.
2. On the EC2 instance, using ​[Ansible](https://github.com/ansible/ansible)​, install ​[Docker](https://docs.docker.com/install/linux/docker-ce/ubuntu/)​ and start a ​[Nginx Docker container](https://hub.docker.com/_/nginx)
that is running version 1.17.6 on port 80.
a. We should be able to connect to this web server from anywhere on the public
internet.
3. Create a ​[GitHub](https://github.com/)​ repo and put all of your code (CloudFormation, Ansible config, etc) in
this repo, with a ​README.md file that contains your documentation explaining how to
use the code in the repo for installing the server.
4. When complete, send the server IP address along with the link to the GitHub repo to
Ken.Cochrane@wexinc.com​ and ​Jered.Philippon@wexin.com

If you have any questions, feel free to reach out to Ken or Jered.

# Setup and Configure Ansible
1. Create a new EC2 Instance 
2. sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update -y
sudo apt-get install ansible -y

## Configure our inventory
sudo nano /etc/ansible/hosts

add the following:
[servers]
server1 ansible_host=52.15.107.235

[servers:vars]
ansible_python_interpreter=/usr/bin/python3

Create an SSH key
ssh-keygen
Answer yes, specify a password if for additional security
cat ~/.ssh/id_rsa.pub and copy to clipboard

#Create Docker Ubuntu Instance in CloudFormation

#SSH into the new Instance for Docker
vi ~/.ssh/authorized_keys

paste the pub key from the previous step at the end of this file and write/quit

You should now be able to run the following command:
ansible all -m ping -u ubuntu

Sample output:
server1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

From ansible run:
ansible-playbook ans-provision.yml
