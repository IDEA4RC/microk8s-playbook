# microk8s-playbook
A playbook to setup microk8s and other capsule dependencies on Ubuntu 22.04 LTS. This setup is suited for single node IDEA4RC capsule deployments and test/development environments.

You will need a provisioner node with ansible that will connect to the provisioned node  that will host the IDEA4RC capsule and set everything up. These two nodes (the provisioner & the provisioned) can happen to be the same machine.

This playbook will do the following:
- install microk8s
- enable several IDEA4RC capsule dependencies like istio, helm, metalLB, prometheus, kiali

## Installation steps
install ansible on the provisioner node:
```
sudo apt install -y ansible
```

generate new ssh keys if not yet available:
```
ssh-keygen
```

copy the generated keys on the node being provisioned:
```
ssh-copy-id user@hostname
```

...And then login and add the remote user to sudoers. Please check the correct group for your Linux distribution. This user must  have sudo privileges passwordlessly. Once that's done simply disconnect:
```
sudo usermod -aG adm $USER
```

> [!TIP]
> If Ansible is still unable to escalate privileges passwordlessly, you can add the following line at the end of your sudoers file, replacing << my_user >> with the desired user name:
> ```
> << my_user >>  ALL=(ALL:ALL) NOPASSWD: ALL
> ```

clone the git repo on the provisioner node:
```
git clone https://github.com/IDEA4RC/microk8s-playbook
```

enter the cloned repo:
```
cd microk8s-playbook/
```

Execute the playbook on the remote host. If you're specifying a single host from the cli, don't forget the comma at the end of the hostname:
```
ansible-playbook -i user@hostname, microk8s_setup.yml
```

> [!IMPORTANT]
> The playbook is going to enable metalLB in our microk8s cluster. This setup requires the user to provide the IP of the VM that is going to be used to access the capsule. The playbook is going to assume that this is the default IPv4 address of the VM. If you need to change this IP, please do so by running the playbook this way:
> ```
> ansible-playbook -i user@hostname, --extra-vars "lb_vip_address=MY_PUBLIC_IP" microk8s_setup.yml
> ```

That's all! From here on, the next steps will the be the deployment of your IDEA4RC capsule (see: https://github.com/IDEA4RC/idea4rc-helm-capsule).
