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
      
## C - Deploy dynamicaly key on remote hosts


    - authorized_key:
      user: "{{ item.user }}"
      key: "{{ item.key }}"
      exclusive: "{{ item.exclusive | default('no') }}"
      state: "{{ item.state | default('present') }}"
      with_items: "{{ ssh_authorized_keys }}"
      
      
      
ss