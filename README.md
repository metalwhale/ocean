# ocean
Ocean is the place where whales live, eat, sleep, and think.

## Prerequisites
- Access the bastion
- Add public key of bastion to each node

## Initial setup
Create a virtual environment:
```bash
sudo apt-get install -y python3-pip
pip3 install -U virtualenv
python3 -m virtualenv --python=$(which python3) ansible
source ansible/bin/activate
```

Clone this repository:
```bash
git clone --recurse-submodules https://github.com/metalwhale/ocean
cd ./ocean/
```

Install requirements for `kubespray` ([reference](https://github.com/kubernetes-sigs/kubespray/blob/master/docs/ansible.md#installing-ansible)):
```bash
cd ./kubernetes/kubespray/
pip install -U -r requirements.txt
cd ../../
```

Install galaxy roles
```bash
ansible-galaxy install -r requirements.yml
```

## Apply playbooks
### Base
Create inventory file and replace the placeholders `IP_ADDRESS` with correct values:
```bash
cp ./ocean/inventory.example.yaml ./ocean/inventory.yaml
vi ./ocean/inventory.yaml
```

Run basic setup:
```bash
cd ./ocean/
ansible-playbook -i inventory.yaml --ask-become-pass base.yml
cd ../
```

### Deploy kubernetes cluster
Create inventory file and replace the placeholders `IP_ADDRESS` with correct values:
```bash
cp ./kubernetes/inventory/hosts.example.yaml ./kubernetes/inventory/hosts.yaml
vi ./kubernetes/inventory/hosts.yaml
```

Deploy cluster:
```bash
cd ./kubernetes/kubespray/
ansible-playbook -i ../inventory --become --become-user=root --ask-become-pass cluster.yml
ansible-playbook -i ../inventory --become --become-user=root --ask-become-pass ../custom/cluster.yml
cd ../../
```

Create storage:
```bash
cd ./ocean/
ansible-playbook -i inventory.yaml --become --become-user=root --ask-become-pass storage.yml
cd ../
```
