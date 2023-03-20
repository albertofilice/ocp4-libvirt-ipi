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


Example Playbook
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

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
