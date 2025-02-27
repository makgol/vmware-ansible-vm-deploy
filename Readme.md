# vmware-ansible-vm-deploy

## Getting started
1. Install Poetry

https://python-poetry.org/docs/
```
curl -sSL https://install.python-poetry.org | python3 -
```

2. Install library
```
poetry install
```

3. Modify all.yml and vars.yml
- **Example**:  
    inventory/group_vars/all.yml
    ```
    # vCenter section
    vcenter_hostname: "192.168.1.1"
    vcenter_username: "administrator@vsphere.local"
    vcenter_password: "password"
    vcenter_datacenter: "dc"
    vcenter_cluster: "cl01"
    
    #folder
    root_folder: "/{{ vcenter_datacenter }}/vm"
    
    #vm section
    vms: >-
      [
        {% for vm_number in range(number_of_vms) %}
        {
          "name": "{{  user }}-{{ vm_name_prefix }}{{ '%03d' |     format(vm_number+1) }}",
          "vm_ip": "{{ vm_start_ip | ansible.utils.ipmath    (vm_number) }}"
        },
        {% endfor %}
      ]
    ```
    *ubuntu/var.yml*
    ```
    ---
    user: "username"
    vm_password: "password"
    vm_datastore: "datastore"
    vm_name_prefix: "ubuntu"
    vm_domain: "example.com"
    vm_spec:
      cpu: 1
      memory: 2
      disk_size_gb: 10
    number_of_vms: 1
    vm_start_ip: "192.168.1.1"
    vm_ip_prefix: "24"
    vm_gateway: "192.168.1.254"
    vm_dns_server: "8.8.8.8"
    vm_port_group: "VM Network"
    folder_name: "ubuntu-folder"
    resource_pool: "ubuntu-pool"
    template_name: "ubuntu-22.04-server-cloudimg-amd64"
    ssh_public_key: "ssh-public-key"
    install_docker: false
    ```
4. Execute deploy playbook
```
poetry run ansible-playbook ./ubuntu/deploy.yml
```
