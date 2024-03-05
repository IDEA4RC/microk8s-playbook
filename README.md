# microk8s-playbook
A playbook to setup micok8s and other capsule dependencies on Ubuntu 22.04 LTS. This setup is suited for single node IDEA4RC capsule deployments and test/development environments.
You will need an ansible node that will connect to the microk8s node and set everything up. These two nodes can happen to be the same machine.

install ansible on the ansible node:
```
sudo apt install -y ansible
```

generate new ssh keys if not yet available:
```
ssh-keygen
```

copy the generated keys on the remote hosts:
```
ssh-copy-id ubuntu@new-capsule
```

add remote user to sudoers, then disconnect:
```
sudo usermod -aG adm $USER
```

clone the git repo on the ansible:
```
git clone https://github.com/IDEA4RC/microk8s-playbook
```

enter the cloned repo:
```
cd microk8s-playbook/
```

Execute the playbook on the remote host. If you're specifying a single host from the cli, don't forget the comma at the end of the hostname:
```
ansible-playbook -vv  -i user@hostname, microk8s_setup.yml
```
