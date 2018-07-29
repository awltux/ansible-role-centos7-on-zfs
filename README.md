# fedora-hosted-docker-cluster-ansible-playbook
Ansible playbook that manages a Fedora hosted Docker cluster

## Setup a Fedora-server:28 instance

Create an 'admin' user, add to 'wheel' group.
Configure sudo to allow NO_PASWD on wheel group.   

This server is the first member of the Docker cluster.
It will eventually be re-imaged once cluster is properly initialised.

```
git clone https://github.com/awltux/fedora-hosted-docker-cluster-ansible-playbook.git
cd fedora-hosted-docker-cluster-ansible-playbook/ansible
ansible-playbook setup-bootstrap-host.yml
```
