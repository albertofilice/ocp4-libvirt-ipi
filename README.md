Ocp4 Libvirt IPI
=========

An ansible playbook for install Openshift using Libvirt IPI 

Disclaimer
=========

This environment has been created for the sole purpose of providing an easy to deploy and consume Red Hat OpenShift Container Platform 4 environment.

Use it at your own pleasure and risk!

Requirements
------------

Ansible
Git
pull-secrets from https://cloud.redhat.com/openshift/install/metal/user-provisioned


Role Variables
--------------

All Variable for playbook can be found in main.yml and install-config.yml

|variable name | description	| Default |
|--------------|----|----|
| network_interface |   Network Inteface Name, you can see with command: $ nmcli con show     |  eth0 |
| libvirt_api_endpoint |  ip of libvirt on the virt interface, usually ip of the virbr0 bridge interface    |  192.168.122.1 |
| workdir |   Default user HOME     |  ansible_env.HOME |
| nestedvirtualization |   Nested virtualization lets you run virtual machine (VM) instances inside of other VMs     |  true |
| pool_location |   Storage pool location   |  /var/lib/libvirt/openshift-images |
| nfs_export_path |   Storage nfs path |  /var/lib/libvirt/openshift-nfs |
| nfs_storage | Enable or Disable NFS |  true |
| ocp_install_file_path | Path of install-config.yml |  install-config.yml |
| ocp_install_path |   Default user HOME/ocp         |  {{ workdir }}/ocp |
| ocp_mirror |    |  https://mirror.openshift.com/pub/openshift-v4/clients/ocp |
| ocp_openshift_installer_repo |        |  https://github.com/openshift/installer |
| ocp_install_install_release_image_registry |    for okd use https://quay.io/openshift/okd    |  quay.io/openshift-release-dev/ocp-release |
| ocp_release |    for okd use 4.11.0-0.okd-2023-01-14-152430	   |  4.11.20 |
| ocp_master_memory |        |  24 |
| ocp_master_cpu |        |  8 |
| ocp_master_disk |        |  150 |
| ocp_worker_memory |        |  32 | 
| ocp_worker_cpu |        |  8 |
| ocp_worker_disk |        |  150 |
| ocp_api_vip |        |  192.168.126.11 |
| ocp_apps_vip |        |  192.168.126.51 |
| ocp_cluster_net_gw |  default gateway for machineNetwork cidr |  192.168.126.1 |
| masters_schedulable |        |  false
| clusteradmin |   default htpasswd user     |  |
| clusterpassword |    default htpasswd password     |  |
| letsencrypt_account_email |    required cloudflare   |  |
| cloudflare_account_email |    if this variable is present on main.yml enable dns and certificate configuration with cloudflare |  |
| cloudflare_account_api_token |   required cloudflare     |  |

Run Playbook
----------------

Run
```bash
git clone https://github.com/albertofilice/ocp4-libvirt-ipi.git
cd ocp4-libvirt-ipi
#Change install-config.yml and main.yml variable.
ansible-playbook main.yml
```

OKD
----------------

We haven't been able to test installing okd at this time, so for okd the playbook may not work.

License
-------

MIT
