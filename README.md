Ocp4 Libvirt IPI
=========

An ansible playbook for install Openshift using Libvirt IPI 

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
| network_interface |        |  eth0
| libvirt_api_endpoint |        |  192.168.122.1
| workdir |        |  ansible_env.HOME
| nestedvirtualization |        |  true
| pool_location |        |  /var/lib/libvirt/openshift-images
| nfs_export_path |        |  /var/lib/libvirt/openshift-nfs
| nfs_storage |        |  true
| ocp_install_file_path |        |  install-config.yml
| ocp_install_path |        |  {{ workdir }}/ocp
| ocp_mirror |        |  https://mirror.openshift.com/pub/openshift-v4/clients/ocp
| ocp_openshift_installer_repo |        |  https://github.com/openshift/installer
| ocp_install_install_release_image_registry |        |  quay.io/openshift-release-dev/ocp-release
| ocp_release |        |  4.11.20
| ocp_master_memory |        |  24
| ocp_master_cpu |        |  8
| ocp_master_disk |        |  150
| ocp_worker_memory |        |  32
| ocp_worker_cpu |        |  8
| ocp_worker_disk |        |  150
| ocp_api_vip |        |  192.168.126.11
| ocp_apps_vip |        |  192.168.126.51
| ocp_cluster_net_gw |        |  192.168.126.1
| masters_schedulable |        |  false
| clusteradmin |        |  <CHANGEME clusteradmin>
| clusterpassword |        |  <CHANGEME clusterpassword>
| letsencrypt_account_email |        |  <CHANGEME letsencrypt_account_email>
| cloudflare_account_email |        |  <CHANGEME cloudflare_account_email>
| cloudflare_account_api_token |        |  <CHANGEME cloudflare_account_api_token>

Run Playbook
----------------

Run
```bash
git clone https://github.com/albertofilice/ocp4-libvirt-ipi.git
cd ocp4-libvirt-ipi
#Change install-config.yml and main.yml variable.
ansible-playbook main.yml
```

License
-------

MIT
