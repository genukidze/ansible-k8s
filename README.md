# Ansible k3s deployment.

This playbook is created to automatically deploy k3s 1-node cluster.

To start playbook, you should set target machine  ip and private ssh-key in hosts fole and simply run

`ansible-playbook playbook.yml`

in your terminal.

## Deployment steps

1) Check ansible version on your PC via `ansible --version` command. To execute command you need Ansible 2.15.5
2) Generate ssh-keys. Public key must be present on the target machine, and private key must be kept on your playbook-executor host.
3) Check availability on the target machine with `ssh -i "your-private-key.pem" 111.111.111.111` command. Replace private key and ip with your values.
4) Add target machine to the `hosts` file. For example: `ubuntu@3.235.105.118 ansible_ssh_private_key_file=/home/user/.ssh/my-private-key.pem`, where ubuntu is username with sudo rights on target machine, and ansible_ssh_private_key_file - path to your private key file.

## Environment Variables

Despite simplicity in use of Playbook, you should remind some variables:

`hostpath_ebs` and `lvm_ebs` in openEBS-setup role are boolean values, and you should choose between them. If you set `lvm_ebs: true` you **must** set `disk_name` and `vg_name` variables. 

In prerequisites-setup you should set `helm_version_number` to a version you need.

In k3s-setup you should set `k3s_version` variable to define a needed k3s version.

Also you should set up `keycloak_chart_version` in helms-setup role and `openebs_version` in openEBS-setup role.

Other variables are intuitive to use.
