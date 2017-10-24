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

## 0 - Generate key

    - name: Generate key pair
          shell: "ssh-keygen -b 2048 -t rsa -f {{ remote_key_remote_path }}/id_rsa -q -N /dev/null"
          args:
            creates: "{{ remote_key_remote_path }}/id_rsa"
        
##  A - Deploy an SSH key with Ansible

 	- name: Deploy public key on current host {{ ansible_host }} for user {{ ansible_user }}
      authorized_key:
        user: "{{ ansible_user }}"
        key: "{{ hostvars['master'].master_public_key.stdout }}"
        exclusive: "no"
        state: "present"


## B - Deploy an SSH key but it is the only one for the remote (exclusive: yes)

    - name: Deploy public key on current host {{ ansible_host }} for user {{ ansible_user }}
      authorized_key:
        user: "{{ ansible_user }}"
        key: "{{ hostvars['master'].master_public_key.stdout }}"
        exclusive: "yes"
        state: "present"
      
      
## C - Remove an SSH key (not existing in this program)

    - authorized_key:
      user: "{{ ansible_user }}"
      key:  "{{ hostvars['master'].master_public_key.stdout }}"
      state: absent

## D - Backup key on the Master
    - name: Copy ssh file to remote dir
      copy: src={{ remote_key_remote_path }}/{{item}}  dest={{ remote_key_master_path }}/ remote_src=no directory_mode=yes
      with_items:
      - "id_rsa"
      - "id_rsa.pub"

