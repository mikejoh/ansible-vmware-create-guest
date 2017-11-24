# Create a VMware guest with Ansible
I created this project as an excuse to learn more about Ansible, nothing advanced but i'll be using *roles* to structure the project and also *vault*. I've used this in my home lab environment.

## Notes
* I've hardcoded the ISO image in the task that i want to boot up with (Debian 9 in this case).
* I've not fully automated this all the way meaning that i don't have a preseed file or URL, for fun i might add a task that spins up a docker container feeding the preseed file and then shutting it down when we're done.

## Setup

1. Install Ansible, i created a new virtualenv (via Python) and then installed Ansible, also don't forget to use the python 2.7 interpreter with you virtualenv.
```
virtualenv --python=$(which python2) <name>
source <name>/bin/activate
pip install ansible
```
2. Clone this repo into a subdirectory to your newly created virtualenv directory.
3. Create a vault password file containing a super secret password (you might not want to 'echo' that into the file as below).
```
touch ~/.vault_pass.txt
chmod 600 ~/.vault_pass.txt
echo "supersecret" >> ~/.vault_pass.txt
```
4. Create a **vault.yml** file in the directory containing the other Ansible files, in this file add the username and password as variables, these credentials will be used to connect to your ESXi host and create VMs.

Contents of **vault.yml**:
```
username: root
password: bringtherain
```

Encrypt the **vault.yml** file, since we have a variable in **ansible.cfg** that points at your local vault password file you only have to run the following:
```
ansible-vault encrypt vault.yml
```

5. Change the host(s) in the inventory-file so that it reflects your environment.
6. Run the playbook, there's no need to provide the inventory-file, we have configured a default inventory in the ansible.cfg file.
```
ansible-playbook create-guest.yml
```
