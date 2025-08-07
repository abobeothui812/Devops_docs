# Ansible EE Setup Guide

# Required Packages
- **Podman/Docker**
- **Python3**
- **Python3-pip**


# Step 1 : Install ansible-navigator
```bash
pip3 install ansible-navigator
```
Installing ansible-navigator lets you run EEs on the command line. It includes the ansible-builder package to build EEs.

If you want to build EEs without testing, install only ansible-builder:

```bash
pip3 install ansible-builder
```

Verify your environment with the following commands
```bash
ansible-navigator --version
ansible-builder --version
```
# Step 2 : Build your Execution Environment

Create a project folder on your filesystem.
```bash
mkdir my_first_ee && cd my_first_ee
```
Create a execution-environment.yml file that specifies dependencies to include in the image.

```ini
version: 3

images:
  base_image:
    name: registry.fedoraproject.org/fedora:42

dependencies:
  python_interpreter:
    package_system: python3
  ansible_core:
    package_pip: ansible-core
  ansible_runner:
    package_pip: ansible-runner
  system:
  - openssh-clients
  - sshpass
  galaxy:
    collections:
    - name: community.postgresql
```

Build a EE container image called postgresql_ee.

If you use docker, add the --container-runtime docker argument.
```bash
ansible-builder build --tag postgresql_ee
```

# Step 3 : Running your EE
Create a test_localhost.yml playbook.
```ini
- name: Gather and print local facts
  hosts: localhost
  become: true
  gather_facts: true
  tasks:

   - name: Print facts
     ansible.builtin.debug:
      var: ansible_facts
```

Run the playbook inside the postgresql_ee EE.
```bash
ansible-navigator run test_localhost.yml --execution-environment-image postgresql_ee --mode stdout --pull-policy missing --container-options='--user=0'
```
