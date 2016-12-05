# ����Kubernetes�ڲ���֤����

> ����ansible����k8s���ڲ���֤���Ƚϼ򵥵���ͨ��CSV����֤��ʽ��

* ���ȴ�����play����ָ��ִ��task��Ŀ����Ͷ���
```ssh
- hosts: masters
  roles:
    - k8s-ssh
- hosts: minions
  roles:
    - k8s-ssh
```
* ����task�б�

```ssh
# ���ȴ���api server�Ļ�����֤�ļ�����������CA��api server��֤��
# �˲��轫��ִ�нű�init-ssl-ca.sh��init-ssl.sh��ͬ���
- name: Generate root CA
  local_action: script init-ssl-ca.sh /tmp/ssl
  sudo: false
  run_once: true

- name: Generate admin key/cert
  local_action: script init-ssl.sh /tmp/ssl admin kube-admin
  sudo: false
  run_once: true
  
- name: provision masters ssl
  local_action: script init-ssl.sh /tmp/ssl apiserver kube-apiserver-{{ hostvars[item]['ansible_default_ipv4']['address'] }} {% for host in groups['masters'] %}IP.{{ loop.index }}={{ hostvars[host]['ansible_default_ipv4']['address'] }}{% if not loop.last %},{% else %},IP.{{ loop.index+1 }}={{ k8s_service_ip }}{% endif %}{% endfor %}
  sudo: false
  with_items: "{{ groups['masters'] }}"
  run_once: true

- name: provision works ssl
  local_action: script init-ssl.sh /tmp/ssl worker-{{ ansible_default_ipv4.address }} kube-worker-{{ ansible_default_ipv4.address }} IP.1={{ ansible_default_ipv4.address }}
  sudo: false
```
> ������������������֤������ļ����������£�
> ca.key���Լ����ɵ�CA��˽Կ������ģ��һ��CA
> ca.crt�����Լ���˽Կ��ǩ����CA֤��
> server.key��api server��˽Կ����������api server��https
> server.csr��api server��֤�������ļ�����������api server��֤��
> server.crt�����Լ�ģ���CAǩ����api server��֤�飬��������api server��https

> ����������֤���ļ�������ÿ���ڵ㣬����master��minion�ڵ㡣
> Ȼ�󴴽��������.

```ssh
- name: create /etc/kubernetes directory
  file: path=/etc/kubernetes state=directory mode=0755

- name: copy ssl directory
  copy: src=/tmp/ssl dest=/etc/kubernetes

- name: write worker-kubeconfig.yaml
  template: src=worker-kubeconfig.yaml.j2 dest=/etc/kubernetes/worker-kubeconfig.yaml
```
> ���ǻ����ڴ���master��minion�ڵ�ʱ���������
