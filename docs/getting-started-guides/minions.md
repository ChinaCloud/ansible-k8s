# ����Kubernetes Minion�ڵ�

* ����play��ָ��ִ��task��Ŀ����Ͷ���
```ssh
- hosts: minions
  roles:
    - minions
```

* ����task�б�

```ssh
# �����봴��master�ڵ��ֻ࣬�Ǵ���minion��ʱ����Ҫ����kube-proxy����
- name: copy kube-proxy
  copy: src=kube-proxy-{{ k8s_version }} dest=/opt/bin/kube-proxy mode=0755
```

```ssh
# ִ��playbook������minions�ڵ�
ansible-playbook master.yml -i inventory
```