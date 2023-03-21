Disclaimer
=========

This environment has been created for the sole purpose of providing an easy to deploy and consume Red Hat OpenShift Container Platform 4 environment.

Use it at your own pleasure and risk!

Ocp4 Libvirt IPI
=========

An ansible playbook for install Openshift using Libvirt IPI.

![ocp4-ipi-libvirt](https://user-images.githubusercontent.com/35273403/226682889-f1b1eb20-e4c5-4f9c-b559-a93cc17fa032.png)

I wrote this playbook to quickly and easily install development environments, I used it on hetzner and contabo with dedicated servers mainly on rocky linux operating system, but can also be used on NUC and Homelab.

## Advice

- I highly recommend servers with ssd disks to avoid etcd slowness problems and api response.
- I configured my server with two IPV4 and 2 IPV6 addresses. One I use for ssh, management and api access, the other for web access to the console and applications. I configured both ip addresses on the same interface for convenience and managed the rules via firewalld, below is an example:

```bash

firewall-cmd --zone=public --add-rich-rule='rule family=ipv4 destination address=<PUBLIC IP>/24 service name=ssh reject' --permanent  
firewall-cmd --zone=public --add-rich-rule='rule family=ipv4 destination address=<PUBLIC IP>/24 service name=cockpit reject' --permanent  
firewall-cmd --zone=public --add-rich-rule='rule family=ipv4 destination address=<MANAGEMENT IP>/24 service name=http reject' --permanent  
firewall-cmd --zone=public --add-rich-rule='rule family=ipv4 destination address=<MANAGEMENT IP>/24 service name=https reject' --permanent  
firewall-cmd --zone=public --add-rich-rule='rule family=ipv4 destination address=<PUBLIC IP>/24 port port=9090 protocol=tcp  reject' --permanent
firewall-cmd --zone=public --add-rich-rule='rule family=ipv4 destination address=<PUBLIC IP>/24 port port=6443 protocol=tcp  reject' --permanent
firewall-cmd --zone=public --add-rich-rule='rule family=ipv4 destination address=<MANAGEMENT IP>/24 port port=80 protocol=tcp  reject' --permanent
firewall-cmd --zone=public --add-rich-rule='rule family=ipv4 destination address=<MANAGEMENT IP>/24 port port=443 protocol=tcp  reject' --permanent

```

Cluster Options
=========

You can install different types of clusters, for example:

- SNO (Single Node OpenShift, just a Control Plane!).
- Three-node cluster (3 Control Plane).

Is it possible to deploy zero compute machines in a bare metal cluster that consists of three control plane machines only, with flag
```yaml
masters_schedulable: true
```
- Normal Provisioning (3 Control Plane, N Compute).

In this case it is always better not to have schedulable control planes.

Libvirt How to
------------
IPI support for the libvirt platform is not yet supported, therefore only the following option is available in the install-config.yaml when using libvirt:

```yaml
platform:
  libvirt:
    URI: qemu+tcp://192.168.122.1/system
    network:
      if: tt0
```

This playbook automates the steps described [here](https://github.com/openshift/installer/blob/master/docs/dev/libvirt/README.md) and implements various customizations to make the cluster usable and ready to go.

In particular:

- nestedvirtualization useful for example for installing openshift cnv
- nfs with autoprovisioner [Link](https://github.com/kubernetes-sigs/nfs-subdir-external-provisioner/blob/master/README.md)
- VM Sizing as Memory, CPU and Disk
- Cloudflare Support and Letsencrypt for certificate


Enable nested virtualization at host
------------

1. Check CPU Information

```bash
cat /proc/cpuinfo | grep "vendor_id" | head -n 1
```

2. edit `/etc/modprobe.d/kvm.conf`

```bash
options kvm_intel nested=1
# or 
options kvm_amd nested=1
```
Reboot your host.

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
