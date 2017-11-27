ovv.lxc
=======

Ansible role to create and start LXC containers on proxmox.

Installation
------------

To install this roles clone it into your roles directory.

```bash
$ git clone https://github.com/ovv/ansible-role-proxmox-lxc.git ovv.lxc
```

If your playbook already reside inside a git repository you can clone it by using git submodules.

```bash
$ git submodule add -b master https://github.com/ovv/ansible-role-proxmox-lxc.git ovv.lxc
```

Role Variables
--------------

* `proxmox_api_user`: Proxmox user.
* `proxmox_api_password`: Proxmox password.
* `proxmox_api_host`: Proxmox api host.
* `proxmox_node`: Proxmox node where container will be created.
* `ct_hostname`: Container hostname.
* `ct_key`: SSH public key to install in container.
* `ct_template`: Container template.
* `ct_network`: Container network (default to `{"net0":"name=eth0,bridge=vmbr0,ip=dhcp"}`).

Some other variable and their defaults are located in [defaults](defaults/main.yml).


Example Playbook
----------------

```yml
- hosts: lxc
  roles:
    - ovv.lxc
  vars:
    proxmox_api_user: admin
    proxmox_api_password: password
    proxmox_api_host: proxmox.example.com
    proxmox_node: proxmox-01
    ct_hostname: mycontainer
    ct_key: "{{ lookup('file', '~/.ssh/id_sa.pub') }}"
    ct_template: local:vztmpl/debian-9.0-standard_9.0-2_amd64.tar.gz    
```

License
-------

MIT
