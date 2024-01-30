# Ansible k3s deployment.

This playbook is created to automatically deploy k3s 1-node cluster.

## Deployment steps

1) Check ansible version on your PC via `ansible --version` command. To execute command you need Ansible 2.15.5. If you don't have Ansible, you should install it via [this guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).
    a) If your OS is Ubuntu, you can install Ansible executing these commands: 
       `sudo apt-add-repository ppa:ansible/ansible`
       `sudo apt update`
       `sudo apt install ansible`
2) Use your own ssh-keys/Generate ssh-keys. Public key must be present on the target machine, and private key must be kept on your playbook-executor host. If you don't know how to generate ssh keys - follow [this guide](https://docs.oracle.com/en/cloud/cloud-at-customer/occ-get-started/generate-ssh-key-pair.html#GUID-8B9E7FCB-CEA3-4FB3-BF1A-FD3406A2432F).
3) Check availability on the target machine with `ssh -i "your-private-key.pem" machine-user@111.111.111.111` command. Replace private key and ip with your values. If you are asked , 
4) Add target machine to the `hosts` file. For example: `ubuntu@111.111.111.111 ansible_ssh_private_key_file=/home/user/.ssh/my-private-key.pem`, where:
   ubuntu is username with sudo rights on target machine;
   111.111.111.111 - ip address of targen machine;
   ansible_ssh_private_key_file is a path to your private key file.
5) Now you can execute playbook with `ansible-playbook playbook.yml` command in the terminal, being in the directory where playbook is located.

## Environment Variables

Despite simplicity in use of the Playbook, you should remind some variables:

`hostpath_ebs` and `lvm_ebs` in openEBS-setup role are boolean values, and you should choose between them. If you set `lvm_ebs: true` you **must** set `disk_name` and `vg_name` variables. 

In prerequisites-setup you should set `helm_version_number` to a version you need.

In k3s-setup you should set `k3s_version` variable to define a needed k3s version.

Also you should set up `keycloak_chart_version` in helms-setup role and `openebs_version` in openEBS-setup role.


Other variables are intuitive to use.
