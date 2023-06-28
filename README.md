# Flask_DA

<img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/flask/flask-original-wordmark.svg" width="40" height="40" /><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/docker/docker-original.svg" width="40" height="40" /><img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/ansible/ansible-original-wordmark.svg" width="40" height="40" />
          

Flask, Docker, Ansible

A simple (very simple) application to learn to use Docker and Ansible for the future uses from my repositories.

## How to

Install ansible:

    apt install ansible

Verify:

    ansible --version

Running:

    ansible-playbook --ask-become-pass playbook.yml

    ansible-playbook playbook.yml

Finally:

    http://localhost:5099/


## Step by Step

Hello people, in this section we have a step-by-step guided tutorial

### What you need

The requirements for following are:

1. A Linux Laptop/Computer

2. An account in some cloud provider - [I use Oracle Cloud](https://www.oracle.com/br/cloud/free/?intcmp=ohp052322ocift)

3. Your time

### First step - SSH

If you using Linux for the first time, you need to generate a pair of SSH Keygens - [tutorial](https://docs.github.com/pt/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux)

1. Open your terminal (ctrl + alt + t in commom linux)

2. Put the command above

> ssh-keygen -t ed25519 -C "your_email@example.com"

3. In your home, access .ssh directory and locate the SSH public key

> cd /home/your-user/.ssh

> cat <keygen_name>.pub

> <key_type> <public_key> <optional_comment>

For exemple:

> ssh-rsa AAAAB3BzaC1yc2EAAAADAQABAAABAQD9BRwrUiLDki6P0+jZhwsjS2muM......yXDus/5DQ== rsa-key-20201202

4. copy the keygen, we going to use it.


### Oracle cloud instance

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

### Some basic configurations in your Oracle Instance

So, now we access using your local terminal

> ssh ubuntu@<Any_Name_You_Want>

or

> ssh ubuntu@<Your_Instance_IP_Addres>

Choose the best option for you!

1. Now, you're on your Ubuntu server

2. Create a new user to use Ansible, my suggestion is a 'ansible' user, LOL

> sudo adduser ansible

3. Set the privileges in sudoers...... first access the sudoers

> sudo nano /etc/sudoers

4. Locate the User privilege specification, there is a specification to root user, just like that:

root ALL=(ALL:ALL) ALL

5. Add a line after the root line

> ansible ALL=(ALL:ALL) NOPASSWD:ALL

6. In this configuration, we're saying to Linux that, when you need to scale your privileges, just do it, don't question me about the password.

### Other basic configurations in your Oracle instance

So, to make short's steps, where we go...

1. I just copy authorized keys in the  /home/ubuntu/.ssh

> cat /home/ubuntu/.ssh/authorized_keys

Copy this value, and then paste it into this same file in ansible user:

> nano /home/ansible/.ssh/authorized_keys

An observation here: maybe you need to change a user to access the home of another user

2. Now, configure the ssh config, search the 'PasswordAuthentication' and make sure that is just like that:

> PasswordAuthentication no

3. Restart the ssh service

> sudo service ssh restart

### Going to ansible

1. In your local machine, install ansible:

> apt install ansible

2. Configure ansible host's

> sudo nano /etc/ansible/hosts

    [oracle]
    #IP ou dominio
    Any_Name_You_Want_of_Your_Oracle_Instance

    [oracle:vars]
    ansible_user=ansible

3. Test to verify

> ansible oracle -m ping

You need receive a successful response: ping pong.







## Contact

Lucas Lima Fernandes

lucas.lfernandes@live.com

https://www.linkedin.com/in/lucaslimafernandes/

