# PAN-OS Bootstrap KVM

This is a small playbook / role to generate a bootstrap archive suitable for
deploying PAN-OS VM-Series on KVM. This is set up to make it easy to build and deploy
many VNFs.

## Preparation

Modify the `group_vars/all.yml` file to include your list of VNFs. Then run the playbook
to build all the ISO files, one for each. The following example will build two ISOs
for two VNFs named `panos01` and `panos02`. You may add additional sections as needed
for as many VNFs as you need.

```yaml
vnfs:
  - hostname: panos01
    ip_address: 10.0.0.11
    netmask: 255.255.255.0
    default_gateway: 10.0.0.1
    vm_auth_key: some_auth_key_from_panorama
    panorama_server: 10.0.0.253
    template_name: template_name
    device_group: staging
    dns_primary: 8.8.8.8
    dns_secondary: 4.4.4.4
    auth_code: VM Auth Code
    admin_username: notadmin
    admin_password: SUPER SECRET
  - hostname: panos02
    ip_address: 10.0.0.12
    netmask: 255.255.255.0
    default_gateway: 10.0.0.1
    vm_auth_key: some_auth_key_from_panorama
    panorama_server: 10.0.0.253
    template_name: template_name
    device_group: staging
    dns_primary: 8.8.8.8
    dns_secondary: 4.4.4.4
    auth_code: VM Auth Code
    admin_username: notadmin
    admin_password: SUPER SECRET
```

You may optionally define the `auto_mac_detect` setting to a `yes` or `no` value. If it is not defined it will default to `yes`. This corresponds to the "Use Hypervisor Assigned MAC Addresses" setting under Device | Management | General Settings in the PanOS UI.

By default, all ISO files will be built and stored in the `/tmp` directory and named
according to the `ip_address` given to each VNF.

## Running this playbook

`ansible-playbook site.yml`
