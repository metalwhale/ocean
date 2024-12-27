# ocean
Ocean is the place where whales live, eat, sleep, and think.

## Prerequisites
- Access the bastion
- Add the bastion's public key to each node

## Initial setup
Install `virtualenv`:
```bash
sudo apt-get update -y
sudo apt-get install -y pipx
pipx install virtualenv
pipx ensurepath
```

Create a virtual environment:
```bash
mkdir ./Ocean
cd ./Ocean/
virtualenv ansible
source ansible/bin/activate
```

Clone this repository:
```bash
git clone --recurse-submodules https://github.com/metalwhale/ocean
cd ./ocean/
```

[Install requirements](https://github.com/kubernetes-sigs/kubespray/blob/v2.26.0/docs/ansible/ansible.md#installing-ansible) for `kubespray`:
```bash
cd ./kubernetes/kubespray/
pip install -U -r requirements.txt
cd ../../
```

Install roles:
```bash
ansible-galaxy install -r requirements.yml
```
