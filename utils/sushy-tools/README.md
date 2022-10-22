# Sushy Tools installation automation

An Ansible playbook to automate the installation of the Sushy Tools package
and the service configuration.

## Installation
To install and configure Sushy Tools on the host run the playbook:
```
$ ansible-playbook playbook.yaml
```
The automation implies that the users invoking it can escalate privileges without password (passwordless sudo).

## Customzing network config
Users can define the default IP address to be used by Sushy to listen to. This address
generally belongs to a virtual bridge associated to one of the libvirt networks (which happens
to be the default gateway of the network, thus the `{{ net_prefix }}.1` address).
```
net_prefix: "192.168.100"
net_ip: "{{ net_prefix }}.1"
```

It is possible to also add a custom network for the lab with the `define_custom_net: true` option. 
This task will create a custom bridge and related network config.
The previous `net_prefix` and `net_ip` variables are still mandatory and users must choose from
a free network prefix.

```
# Custom Libvirt network config
define_custom_net: true
net_name: "ztp-lab"
net_uuid: "{{ 99999999 | random | to_uuid }}"
net_bridge: "virbr6"
net_netmask: "255.255.255.0"
net_mac_address: "{{ '52:54:00' | community.general.random_mac }}"
net_dhcp_range_start: "{{ net_prefix }}.2"
net_dhcp_range_end: "{{ net_prefix }}.254"
```

## Customizing authentication
It is possible to define a custom user (default is `admin:redhat`) by passing the desired values
to the `sushy_admin_user` and `sushy_admin_password` variables, like in the following example:
```
sushy_admin_user: "myuser"
sushy_admin_password: "baremeta1Rule5"
```

