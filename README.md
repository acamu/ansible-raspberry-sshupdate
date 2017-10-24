# ansible-raspberry-sshupdate
Create new SSH key and update on remote pi

This script aims to update public key on remote pi by deploying key on them.

[Pre requisites]
- ssh :)

[Execution]
execute the first time (because the pi hasn't keys existing) :

    - ansible-playbook playbook-ssh.yml -i host-firsttime
    
execute the other time :

    - ansible-playbook playbook-ssh.yml -i hosts


[Content of ]

This playbook aim to memorise the last upload of the ssh key to be able to connect the next time it need to update the certificat.

There is four times:

First
Variable update

Second
Manage SSH key 

Three
Upload the current key to remot host

last
Save the current key uploaded successfully 



## Explanation


##  A - Deploy an SSH key with Ansible

 	- authorized_key: user=root key="ssh-rsa AAAA....= pi@ansibleremotehost"


## B - Deploy an SSH key but it is the only one for the remote (exclusive: yes)

    - authorized_key:
      user: "pi"
      key: "ssh-rsa AAAA....= pi@ansibleremotehost"
      exclusive: yes
      
      
## C - Remove an SSH key (not existing in this program)

    - authorized_key:
      user: "pi"
      key: "ssh-rsa AAAA....= pi@ansibleremotehost"
      state: absent




