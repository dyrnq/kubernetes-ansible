# kubernetes-ansible

This repository is for installing kubernetes HA cluster by binary step by step using ansible.
Support docker(ctr-dockerd)、containerd、cri-o.

## install kubernetes by ansible

### prepare

- install virtualbox [ref](https://www.virtualbox.org/manual/ch02.html)
- install vagrant [ref](https://www.vagrantup.com/docs/installation)
- install ansible [ref](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

```bash
vagrant up etcd-1 etcd-2 etcd-3 master-1 master-2 master-3 worker-1 worker-2 worker-3
```

```bash
## need set https_proxy in group_vars/all.yml
ansible-playbook -i inventory/hosts  playbook-grab.yml

## only run once
ansible-playbook -i inventory/hosts  playbook-certs.yml

## only run once
ansible-playbook -i inventory/hosts  playbook-kube-encryption.yml

## only download kubernetes v1.24.4
ansible-playbook -i inventory/hosts -e "kubernetes_ver=v1.24.4"  playbook-grab.yml --tags=path,kubernetes
## only download docker 20.10.17
ansible-playbook -i inventory/hosts -e 'docker_ver=20.10.17'  playbook-grab.yml --tags=path,docker
```
### install etcd
```bash
ansible-playbook -i inventory/hosts  playbook-etcd.yml
```
### kube-master
```bash
ansible-playbook -i inventory/hosts  playbook-kube-master.yml
```
### kube-worker
```bash
ansible-playbook -i inventory/hosts  playbook-kube-worker.yml
## Specify kubernetes version number and cri implementation on worker-2
ansible-playbook -i inventory/hosts -e "run=worker-2" -e "cri=cri-o" -e "kubernetes_ver=v1.24.4" playbook-kube-worker.yml
```
### kube-addons
```bash
ansible-playbook -i inventory/hosts  playbook-kube-addons.yml
```
### ansible-role-nginx
If kubernetes.lb.mode value is "nginx", need clone ansible-role-nginx first by script below.
```bash
mkdir -p ~/.ansible/roles/
cd ~/.ansible/roles/
git clone --depth=1 git@github.com:nginxinc/ansible-role-nginx.git
```

### ansible execute role from command line
```bash
ansible -i inventory/hosts master-1 -m include_role -a name=helm
```

## ref
- <https://github.com/jonashackt/kubernetes-the-ansible-way>
- <https://github.com/easzlab/kubeasz>
- <https://github.com/kairen/kubeadm-ansible>
- <https://github.com/k8sre/k8s>
- <https://github.com/lework/kainstall>
- <https://github.com/qist/k8s>
- <https://github.com/zhangguanzhang/Kubernetes-ansible>
- <https://github.com/geerlingguy/ansible-for-kubernetes>
- <https://github.com/feiyu563/ansible-kubernetes>
- <https://github.com/gjmzj/deploy-k8s-with-ansible>
- <https://github.com/npflan/k8s-ansible>
- <https://github.com/radicalrabbit/k8s-ansible>
- <https://github.com/open-hand/kubeadm-ha>
- <https://github.com/maclarensg/k8s-ha-ansible>
- <https://github.com/cby-chen/Kubernetes>
- <https://github.com/raymond999999/kubernetes-ansible>
- <https://github.com/buxiaomo/kubeasy>