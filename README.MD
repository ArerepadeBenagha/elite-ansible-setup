_________________
ANSIBLE FRAMEWORK
_________________

Ansible architecture is push based 
- Ansible contolled machine/slave
- Ansible controller machine/master
- push based with an ssh-agent on slave machine.
- ssh works on port 2200 or 22 as widely known.
On ansible controller machine ansible will be installed but in the slave machine no ansible software needs to be installed.

---------------------------------
Ansible uses playbooks --> plays -----> tasks -----> action

Ansible is based on python, without python ansible will not owrk because it is the frame work.
Ansible have set of commands and are saved in modules which are written in python.

CONFIGURE
=========

setup on master.
----------------
## Ensure password less connection, how?
sudo nano /etc/ssh/sshd_config
change (password authentication = yes)

# switch from root user to another or a local user.
useradd elitesolutions
passwd  elitesolutions

# Provide root privillege to elitesolutions
sudo nano /etc/sudoers
elitesolutions  ALL=NOPASSWD:  ALL

# restart sshd file to sync changes
systemctl restart sshd

# Install epel repo
sudo amazon-linux-extras install epel

# Install ansible software
yum install ansible -y

# Create Inventory
sudo nano /etc/ansible/hosts
# create node groups

[webserver]
enter private-ip for slave EC2



# <----> on slave machine <--->

## Ensure password less connection, how?
sudo nano /etc/ssh/sshd_config
change (password authentication = yes)

# switch from root user to another or a local user.
useradd elitesolutions
passwd  elitesolutions

# Add user to sudoers file
sudo nano /etc/sudoers
elitesolutions  ALL=NOPASSWD:  ALL

# restart sshd
systemctl restart sshd


# On master and slave switch user to new local user
su - elitesolutions

# create a link between master and slave or agent master
# on master
su - elitesolutions

ssh-keygen # create a key

# Copy key create to slave machne
ssh-copy-id elitesolutions@IP-ADDRESS of EC2-remote machine

# Access slave machine from master
# If configure correctly you will see a connection string to slave machine from master
ssh 'elitesolutions@3.237.201.198'

SETUP IS READY, check connection
--------------------------------
ansible -m ping webserver # test connection

# -m flad stands for module

COMMANDS
--------
# check uptime
ansible -m command -a "uptime" webserver
ansible -m user "name=john password=john" webserver --become   # become authenticates your local user as root user
# check slave machine to check if john exists

# syntax check
ansible-playbook demo-1 --syntax-check

# check how many host gets impacted
ansible-playbook demo-1 --list-host

# How to use roles written by other developers
Go to ansible galaxy: search a key word role e.g jenkins.



/////////////
install python 3
sudo python3 get-pip.py

Install ansible with pip

curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py --user
python3 -m pip install --user paramiko

ansible --version

ansible-playbook datadog.yml -i datadog-inventory.yml --user ubuntu --private-key /home/eliteadmin/.ssh/vault -vvv

////decrypt and encrypt  -----password=vault
ansible-vault encrypt vars/vars.yml