# ansible-devops
Examples from the Ansible for DevOps book

## Prerequisites

Install these system packages before running the project.

- `vagrant`
- `qemu-kvm`
- `dkms`
- `build-essential`
- `linux-headers-$(uname -r)`
- `libvirt-dev` (Debian/Ubuntu)
- `libvirt-daemon-system` (Debian/Ubuntu)
- `libvirt-clients` (Debian/Ubuntu)

## Quick Start

### 1. Create and activate a Python virtual environment

Set up an isolated Python environment so Ansible dependencies do not affect your system Python installation.

```bash
cd /home/macruz/Workspace/ansible-devops
python3 -m venv .venv
source .venv/bin/activate
```

### 2. Install pip and Ansible

Install and update the tooling needed to run Ansible commands and development utilities in this project.

```bash
python -m ensurepip --upgrade
python -m pip install --upgrade pip
python -m pip install ansible
python -m pip install ansible-dev-tools
```

### 3. Start Vagrant VM

Provision and boot the virtual machine defined in `Vagrantfile`.

```bash
vagrant up
```

### 4. Optional: Install Python 3.9 in the VM

If the VM default Python version is older than 3.9, install Python 3.9 so the configured interpreter path is available.

```bash
vagrant ssh
sudo dnf -y install python39
```

### 5. Run Ansible ping

Run a connectivity test to confirm Ansible can reach the target host from your inventory.

```bash
ansible -i hosts.ini ansible_target -m ping
```

Expected result includes `"ping": "pong"` for the target host.
