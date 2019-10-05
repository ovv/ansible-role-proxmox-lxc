# ovv.lxc

Ansible role to create and start LXC containers on proxmox.

## Requirements

This role was tested on Proxmox 5.  As of Ansible 2.8 Proxmox6+ is not supported. Follow the Ansible GitHub issues for more information and status updates: <https://github.com/ansible/ansible/issues/59164>.

The python package ```proxmoxer``` is required on the host you are running ansible from.

## Variables

In order to use this you will need to specify some required variables.  The defaults for the optional variables can be seen in ```defaults/main.yml```.

### Required

```yaml
proxmox_api_host: "proxmox_server_hostname"
proxmox_node: "proxmox_server_hostname"
```

Specify the api server to use and the node to build the container on.  These can be the same (ie you don't have a cluster).

```yaml
proxmox_api_user: proxmox_username
proxmox_api_password: proxmox_password
```

Specify the username and password used to authenticate with proxmox (consider using ansible-vault to encrypt the password).  Note that you will need to specify the authentication type after the username.  The default proxmox user is: ```root@pam```.  If you've created proxmox users you would use: ```username@pve```

```yaml
ct_hostname: "mynewcontainer"
```

Set the hostname of the new container.

```yaml
ct_key: "ssh-ed25519 your_ssh_key123abc456 user@host"
```

Specify an SSH key to use to login to the container.  This is used to login to your new container as the ```root``` user.

```yaml
ct_template: "local:vztmpl/debian-9.0-standard_9.7-1_amd64.tar.gz"
```

Set the template to use for this container.  Note that you can see available containers by running: ```pveam list local``` on the proxmox server (replace local with the storage type that holds your templates).

```yaml
ct_network: '{"net0":"name=eth0,gw=192.168.1.1,ip=192.168.1.10/22,ip6=dhcp,bridge=vmbr0"}' # Use Static IP
ct_network: '{"net0":"name=eth0,ip=dhcp,ip6=dhcp,bridge=vmbr0"}' # Use DHCP
```

```yaml
ct_unprivileged: "yes"
```

Set the container to be unprivileged or not (ok in most cases unless you're working with NFS shares or other system features).

### Optional Variables

You can optionaly set custom resource values for your container.

```yaml
ct_cores: 1
ct_cpus: 1
ct_cpuunits: 1000
ct_storage: local-lvm
ct_disk: 3
ct_memory: 512
ct_onboot: yes
ct_swap: 0
```

## Example Playbook

```yaml
- hosts: all
  user: root
  gather_facts: false
  vars:
    # Required Vars
    proxmox_api_user: "root@pam"
    proxmox_api_password: "mysecretpassword"
    proxmox_api_host: proxmox1
    proxmox_node: proxmox1
    # Optional Vars
    ct_storage: local-lvm
  roles:
    - ansible-role-proxmox-lxc
```

## Notes

This role might not work with your configuration and should be updated accordingly.
