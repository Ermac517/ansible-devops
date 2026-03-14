# ansible-devops
Examples from the Ansible for DevOps book

## Quick Start

### 1. Create and activate a Python virtual environment

```bash
cd /home/macruz/Workspace/ansible-devops
python3 -m venv .venv
source .venv/bin/activate
```

### 2. Install pip and Ansible

```bash
python -m ensurepip --upgrade
python -m pip install --upgrade pip
python -m pip install ansible
python -m pip install ansible-dev-tools
```

### 3. Run Ansible ping

```bash
ansible -i hosts.ini ansible_target -m ping
```

Expected result includes `"ping": "pong"` for the target host.
