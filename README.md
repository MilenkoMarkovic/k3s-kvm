# k3s with primary and secondary nginx or istio ingress

Blog walk-through: https://fabianlee.org/2021/09/12/kubernetes-k3s-cluster-on-ubuntu-using-ansible/

## K3s Cluster Installation

Modify variables for environment
  * vi group_vars/all

Prereq for scripts
  * ansible-playbook install_dependencies.yml

Create guest OS with KVM
  * ansible-playbook playbook_terraform.yml

Install k3s:
  * sudo ls /tmp
  * ansible-playbook playbook_k3s_prereq.yml
  * ansible-playbook playbook_k3s.yml

Install MetalLB and certificates
  * ansible-playbook playbook_metallb.yml
  * ansible-playbook playbook_certs.yml


## Choose between NGINX Ingress
https://fabianlee.org/2021/09/16/kubernetes-k3s-with-multiple-istio-ingress-gateways/

  * ansible-playbook playbook_nginx.yml
  * ansible-playbook playbook_nginx_test.yml

## OR Istio Ingress 

https://fabianlee.org/2021/09/16/kubernetes-k3s-with-multiple-istio-ingress-gateways/

  * ansible-playbook playbook_istio.yml
  * ansible-playbook playbook_istio_test.yml

## Validate Cluster

Validate kubectl locally:
  * export KUBECONFIG=/tmp/k3s-kubeconfig
  * kubectl get services -A

Validate ingress locally:
  * add entries to local /etc/hosts
    192.168.2.143 k3s.local
    192.168.2.144 k3s-secondary.local

  * test nginx ingress
    ./test-nginx-endpoints.sh
  OR
  * test istio ingress
    ./test-istio-endpoints.sh

