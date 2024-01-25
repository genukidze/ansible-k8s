# Ansible k3s deployment.

This playbook is created to automatically deploy k3s 1-node cluster.

To start playbook, you should set target machine  ip and private ssh-key in hosts fole and simply run

`ansible-playbook playbook.yml`

in your terminal.

## Environment Variables

Despite of simplicity in use of Playbook, you should remind some variables

`hostpath_ebs` and `lvm_ebs` in openEBS-setup role are boolean values, and you should choose between them. If you set `lvm_ebs: true` you **must** set `disk_name` and `vg_name` variables. 

Other variables are intuitive to use.
