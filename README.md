# Flask_DA

<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/flask/flask-original-wordmark.svg" width="40" height="40" /><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-original.svg" width="40" height="40" /><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/ansible/ansible-original-wordmark.svg" width="40" height="40" />
          

Flask, Docker, Ansible

A simple (very simple) application to learn to use Docker and Ansible for future uses from my repositories.

This tutorial has been written by me for Linux users, enjoy it.


## Executing in localhost

1. In your local machine, install Ansible:

> apt install ansible

2. Download the playbook_local.yml from this repository

3. Execute

Note: He asks you for your password (to assume a root user)

> ansible-playbook --ask-become-pass playbook_local.yml



## Executing in a cloud server

### Step by Step

Hello people, in this section we have a step-by-step guided tutorial

#### What you need

The requirements for the following are:

1. A Linux Laptop/Computer

2. An account in some cloud provider - [I use Oracle Cloud](https://www.oracle.com/br/cloud/free/?intcmp=ohp052322ocift)

3. Your time

#### First step - SSH

If you are using Linux for the first time, you need to generate a pair of SSH Keygens - [tutorial](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux)

1. Open your terminal (ctrl + alt + t normally in Linux)

2. Put the command above

> ssh-keygen -t ed25519 -C "your_email@example.com"

3. In your home, access .ssh directory and locate the SSH public key

> cd /home/your-user/.ssh

> cat <keygen_name>.pub

> <key_type> <public_key> <optional_comment>

For example:

> ssh-rsa AAAAB3BzaC1yc2EAAAADAQABAAABAQD9BRwrUiLDki6P0+jZhwsjS2muM......yXDus/5DQ== rsa-key-20201202

4. copy the keygen, we going to use it.


#### Oracle cloud instance

Now, we need a compute instance in Oracle Cloud.

1. First, access our Oracle Cloud Console

2. Following the amazing [Oracle Docs](https://docs.oracle.com/pt-br/iaas/Content/GSG/Tasks/launchinginstance.htm#Launching_a_Linux_Instance) create an instance - Please, choose Ubuntu 

One observation here: Choose the 'Paste public keys' option and paste your public SSH keygen that we generate

3. Copy your public IP Address (we're turning easy access to your instance)

4. In your terminal, of your local machine, access your host's archive - I use Nano editor

> sudo nano /etc/hosts

5. You are seeing the host's file, and the first line should be: 127.0.0.1	localhost

6. Now, you need to add a new line: <Your_Instance_IP_Addres>    <Any_Name_You_Want>

Great, now you have an easy access

#### Some basic configurations in your Oracle Instance

So, now we access using your local terminal

> ssh ubuntu@<Any_Name_You_Want>

or

> ssh ubuntu@<Your_Instance_IP_Addres>

Choose the best option for you!

1. Now, you're on your Ubuntu server

2. First, update the repositories

> sudo apt update

3. Create a new user to use Ansible, my suggestion is a 'ansible' user, LOL

> sudo adduser ansible

4. Set the privileges in sudoers...... first access the sudoers

> sudo nano /etc/sudoers

5. Locate the User privilege specification, there is a specification to root user, just like that:

root ALL=(ALL:ALL) ALL

6. Add a line after the root line

> ansible ALL=(ALL:ALL) NOPASSWD:ALL

7. In this configuration, we're saying to Linux that, when you need to scale your privileges, just do it, don't question me about the password.

#### Other basic configurations in your Oracle instance

So, to make short's steps, where we go...

1. I just copy authorized keys in the  /home/ubuntu/.ssh

> cat /home/ubuntu/.ssh/authorized_keys

Copy this value, and then paste it into this same file in Ansible user:

> nano /home/ansible/.ssh/authorized_keys

An observation here: maybe you need to change a user to access the home of another user

2. Now, configure the ssh config on /etc/ssh/sshd_config, search the 'PasswordAuthentication' and make sure that is just like that:

> sudo nano /etc/ssh/sshd_config

> PasswordAuthentication no

3. Restart the ssh service

> sudo service ssh restart

#### Going to Ansible

1. In your local machine, install Ansible:

> apt install ansible

2. Configure Ansible host's

> sudo nano /etc/ansible/hosts

    [oracle]
    #IP ou dominio
    Any_Name_You_Want_of_Your_Oracle_Instance

    [oracle:vars]
    ansible_user=ansible

If, after you installed Ansible, it doesn't create the host's file

So, you need to create the inventory.ini file with the same information, and when you run the Ansible, you have to pass the inventory argument:

> ansible-playbook -i inventory.ini playbook.yml

3. Test to verify

> ansible Any_Name_You_Want_of_Your_Oracle_Instance -m ping

You need to receive a successful response: ping pong.

    Any_Name_You_Want_of_Your_Oracle_Instance | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }

4. Download the playbook_oracle.yml from this repository

5. Execute

> ansible Any_Name_You_Want_of_Your_Oracle_Instance playbook_oracle.yml


#### Testing

1. In the terminal of your instance, type:

> sudo docker ps

And you see the docker running:

| CONTAINER ID 	|   IMAGE   	| COMMAND         	| CREATED        	| STATUS        	| PORTS                  	| NAMES               	|
|:------------:	|:---------:	|-----------------	|----------------	|---------------	|------------------------	|---------------------	|
| 87fc2177167a 	| flask-app 	| "python app.py" 	| 56 minutes ago 	| Up 56 minutes 	| 0.0.0.0:5099->5099/tcp 	| flask-app-container 	|

Sooooo Great, it's running...

Now...

2. Open your internet browser

3. Digit the IP address from our instance with the 5099 port

> 142.250.218.14:5099

The successful message:

> "Hello, Flask app!"

If you don't receive it, means that you need to create network rules... follow [this tutorial to create an ingress rule for your VCN](https://docs.oracle.com/en-us/iaas/developer-tutorials/tutorials/flask-on-ubuntu/01oci-ubuntu-flask-summary.htm#ariaid-title7)


#### Any question?

1. Create a Issue



## Contact

Lucas Lima Fernandes

lucas.lfernandes@live.com

https://www.linkedin.com/in/lucaslimafernandes/

