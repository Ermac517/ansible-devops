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
The VM uses a fixed private IP so Ansible and Vagrant both target the same host.

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

Run a connectivity test to confirm Ansible can reach the Vagrant VM at `172.28.128.10`.

```bash
ansible -i hosts.ini rocky -m ping
```

Expected result includes `"ping": "pong"` for the target host.

### 6. Run the Ansible playbook via Vagrant provisioning

Trigger Vagrant to run `playbook.yml` against the VM using the Ansible provisioner.
The playbook ensures the `chrony` NTP package is installed and the `chronyd` service
is running and enabled on the guest. If the VM was already created before this
network change, recreate it once so the fixed IP is applied.

```bash
vagrant destroy -f
vagrant up
```

```bash
vagrant provision
```

Expected output:

```
PLAY [all] *********************************************************************

TASK [Gathering Facts] *********************************************************
ok: [default]

TASK [Ensure chrony is installed] **********************************************
ok: [default]

TASK [Ensure chronyd service is started and enabled] ***************************
ok: [default]

PLAY RECAP *********************************************************************
default                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

All three tasks should show `ok`, and the recap must have `failed=0`.
