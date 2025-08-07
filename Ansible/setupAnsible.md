# Ansible Setup Guide

# Required Packages
- **Python3**
- **Python3-pipx**


# Step 1 : Install pipx
```bash
sudo apt install pipx
```
# Step 2 : Install ansible

```bash
pipx install --include-deps ansible
```
You can install the minimal ansible-core package:

```bash
pipx install ansible-core
```
To upgrade an existing Ansible installation to the latest released version:

```bash
pipx upgrade --include-injected ansible
```

# Step 3 : Thêm path của ansible để gọi lệnh trong shell
```bash
pipx ensurepath
```
