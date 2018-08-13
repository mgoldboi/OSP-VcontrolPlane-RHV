OSP control plane on RHV playbooks
=========

This is a temporary repo to hold some of the work being done on deploying Openstack control plane on RHV infrastracture

Requirements
------------
Bastion host:
 * Ansible version 2.5 or higher
 * oVirt/RHV Python SDK version 4 or higher
 
RHV:
 * RHV cluster is configured and ready to run ([ovirt-infra role can be used here](https://github.com/oVirt/ovirt-ansible-infra/blob/master/README.md))
 * RHV Hosts network configuration should comply with [OSP Director/undercloud networking requirements](https://access.redhat.com/documentation/en-us/red_hat_openstack_platform/13-beta/html/director_installation_and_usage/chap-planning_your_overcloud#sect-Planning_Networks)
 * RHEL template should be available on RHV target cluster ([ovirt-image-template role can be used here](https://github.com/oVirt/ovirt-ansible-image-template/blob/master/README.md))


Example Playbook
----------------
 * osp_vm_infra.yml - this playbooks creats the VM infrastracture for the undercloud and 3 controllers, including nics, anti-affinity between the VMs, tagging and more. based on [oVirt.vm_infra role](https://github.com/oVirt/ovirt-ansible-vm-infra/blob/master/README.md)
 * Best practice is to move credentials to a vault file

Run: `ansible-playbook osp_vm_infra.yml -e @extra.yml`

extra.yml should look like this:
```
---
#RHV credentials 
engine_url: https://your.rhvm.fqdn/ovirt-engine/api
engine_user: admin@internal
engine_password: changeme

#Undercloud VM
undercould_primary_mac: 00:1a:4a:16:01:f9
undercould_secondery_mac: 00:1a:4a:16:01:fa
undercloud_ip: 10.12.50.148
undercloud_vm_name: my-undercloud
rhel_template: rhel75

#RHSM credentials
rhsm_url: subscription.rhsm.redhat.com
rhsm_username: username
rhsm_password: password

#undercloud.conf
local_ip: 172.16.0.1/24
undercloud_public_vip: 10.12.50.148
undercloud_admin_vip: 172.16.0.11
local_interface: eth1
masquerade_network: 172.16.0.0/24
dhcp_start: 172.16.0.20
dhcp_end: 172.16.0.120
network_cidr: 172.16.0.0/24
network_gateway: 172.16.0.1
inspection_iprange: 172.16.0.150,172.16.0.180

enable_node_discovery: false
enable_ui: true
generate_service_certificate: true
```

License
-------

Apache License 2.0
