# ocean
Ocean is the place where whales live, eat, sleep, and think.

## Prerequisites
- Access the bastion
- Add the bastion's public key to each node

## Initial setup
Install `uv`:
```bash
curl -LsSf https://astral.sh/uv/0.9.3/install.sh | sh
source $HOME/.local/bin/env
```

Create and activate a virtual environment:
```bash
uv venv ocean-venv
source ocean-venv/bin/activate
```

Clone this repository:
```bash
git clone --recurse-submodules https://github.com/metalwhale/ocean
cd ./ocean/
```

[Install requirements](https://github.com/kubernetes-sigs/kubespray/blob/v2.29.0/docs/ansible/ansible.md#installing-ansible) for `kubespray`:
```bash
cd ./kubernetes/kubespray/
uv pip install -r requirements.txt
cd ../../
```

Install roles:
```bash
ansible-galaxy install -r requirements.yml
```

## Running playbooks
### Forming Ocean
Create an inventory file and replace the placeholders with correct values:
- <code>${NODE<b>x</b>_IP_ADDRESS}</code>s: IP addresses of each node
```bash
cd ./ocean/
cp ./inventory.yaml.example ./inventory.yaml
vi ./inventory.yaml
```

Run basic setup:
```bash
ansible-playbook -i inventory.yaml --ask-become-pass base.yml
cd ../
```

### Deploy Kubernetes cluster
Create an inventory file and replace the placeholders with correct values:
- <code>${NODE<b>x</b>_IP_ADDRESS}</code>s: IP addresses of each node
```bash
cd ./kubernetes/
cp ./inventory/inventory.ini.example ./inventory/inventory.ini
vi ./inventory/inventory.ini
```

Deploy Wave cluster:
```bash
cd ./kubespray/
ansible-playbook -i ../inventory/inventory.ini --become --become-user=root --ask-become-pass cluster.yml
ansible-playbook -i ../inventory/inventory.ini --become --become-user=root --ask-become-pass ../wave/cluster.yml
cd ../../
```
