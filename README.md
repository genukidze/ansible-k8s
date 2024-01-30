# Ansible k3s deployment.

This playbook is created to automatically deploy k3s 1-node cluster.


## Repository structure 

```
repository
│   .gitignore                              .gitignore file
│   playbook.yml                            Ansible playbook                                        
│   README.md                               This Readme
│   hosts                                   Hosts file where connectivity to the remote machine should be set up
│   ansible.cfg                             Ansible configuration file
└───roles
│   └───helms-setup                         Keycloak installation role
│   │   └───defaults
│   │   │      └───main.yml                 Keycloak values are located here
│   │   └───tasks
│   │   │      └───main.yml                 Keycloak installation script is located here
│   └───k3s-setup                           K3s installation role
│   │   └───defaults                        K3s installation values are located here
│   │   │   └───main.yml                    
│   │   └───tasks
│   │   │   └───main.yml                    K3s installation task is located here
│   └───openEBS-setup                       openEBS installation role
│   │   └───defaults                        openEBS installation values are located here
│   │   │   └───main.yml                    
│   │   └───tasks
│   │   │   └───main.yml                    openEBS installation task is located here
│   └───prerequisites-setup                 prerequisites installation role
│   │   └───defaults                        prerequisites installation values are located here
│   │   │   └───main.yml                    
│   │   └───tasks
│   │   │   └───main.yml                    prerequisites installation task is located here
```
## Deployment steps

1) Check ansible version on your PC via `ansible --version` command. To execute command you need Ansible 2.15.5+. If you don't have Ansible, you should install it via [this guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).
    + If your OS is Ubuntu, you can install Ansible executing these commands: 
      ```bash
      sudo apt-add-repository ppa:ansible/ansible
      ```
      ```bash
      sudo apt update
      ```
      ```bash
      sudo apt install ansible
      ```
2) Check availability on the target machine using this command:
   ```bash
   ssh -o StrictHostKeyChecking=no -i "your-private-key.pem" machine-user@111.111.111.111
   ``` 
   Use your own ssh-keys. Public key must be present on the target machine, and private key must be kept on your playbook-executor host.  Replace private key, machine-user and ip with your own values.
3) If you don't have your own ssh-keys, you should generate them and then check connectivity. If you don't know how to generate ssh keys - follow this guide:
   + Install openssh-client:
   ```bash
   sudo apt-get install openssh-client
   ```
   + Generate ssh-keys:
   ```bash
   ssh-keygen -b 2048 -t rsa
   ```
   + Add your public key to authorized_keys:
   ```bash
   cat your_public_key_file >> ~/.ssh/authorized_keys
   ```

4) Add target machine to the `hosts` file, which is located in the playbook folder. You need to edit this line: `ubuntu@111.111.111.111 ansible_ssh_private_key_file=/home/user/.ssh/my-private-key.pem`, where:
   + ubuntu is username with sudo rights on target machine;
   + 111.111.111.111 - ip address of targen machine;
   + ansible_ssh_private_key_file is a path to your private key file.
5) Now you can execute playbook with `ansible-playbook playbook.yml` command in the terminal, being in the directory where playbook is located.

## Environment Variables

Despite simplicity in use of the Playbook, you should remind some variables:

`hostpath_ebs` and `lvm_ebs` in openEBS-setup role are boolean values, and you should choose between them. If you set `lvm_ebs: true` you **must** set `disk_name` and `vg_name` variables. 

In prerequisites-setup you should set `helm_version_number` to a version you need.

In k3s-setup you should set `k3s_version` variable to define a needed k3s version.

Also you should set up `keycloak_chart_version` in helms-setup role and `openebs_version` in openEBS-setup role.

Other variables:

helms-setup role:

| Variable | Meaning                                |
|--------|----------------------------------------|
| helm_keycloak_repo_url | Repository where helm chart is located |
| bitnami_repo_url | Bitnami repository                     |
| kubeconfig_path       | Kubeconfig path on the target machine  |
| keycloak_chart_version       | Desired keycloak chart version         |

k3s-setup role:

| Variable | Meaning                               |
|--------|---------------------------------------|
| kubeconfig_path | Kubeconfig path on the target machine |
| k3s_version| Desired k3s version                   |

openEBS-setup role:

| Variable          | Meaning                                                        |
|-------------------|----------------------------------------------------------------|
| disk_name         | Name of the disk which you would like to use for openEBS       |
| vg_name           | Desired volume group name                                      |
| kubeconfig_path   | Kubeconfig path on the target machine                          |
| hostpath_ebs      | True or false. Whether you like to install hostpath ebs or not |
| lvm_ebs           | True or false. Whether you like to install lvm ebs or not      |
| requested_storage | Desired storage for openEBS                                    |
| openebs_version   | Desired openEBS chart version                                  |

prerequisites-setup role:

| Variable | Meaning               |
|--------|-----------------------|
| helm_install_url | Helm installation url |
| helm_version_number| Helm version to use   |

