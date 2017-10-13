# ansible-raspberry-sshupdate
Create new SSH key and update on remote pi

This script aims to update private key on remote pi by deploying key on them.


[Pre requisites]
- ssh :)
- cc


## A - Deploy a SSH key with Ansible

 	- authorized_key: user=root key="ssh-rsa AAAA....= pi@ansibleremotehost"


## B - Deploy a SSH key but it is the only one for the remote

    - authorized_key:
      user: "pi"
      key: "ssh-rsa AAAA....= pi@ansibleremotehost"
      exclusive: yes