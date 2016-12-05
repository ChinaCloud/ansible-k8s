# ����ETCD��Ⱥ

* ���ȴ���play��ָ��ִ��task��Ŀ����Ͷ���
```ssh
- hosts: etcd
  roles:
    - etcd
```
* ���������б�

```ssh
# ����etcd��ִ���ļ��Ĵ��·��
- name: create /opt/bin directory
  file: path=/opt/bin state=directory mode=0755
  
# �ϴ�etcdctl��ִ���ļ���Ŀ��·��
- name: copy etcd
  copy: src=etcd-{{ etcd_version }} dest=/opt/bin/etcd mode=0755
- name: copy etcdctl
  copy: src=etcdctl-{{ etcd_version }} dest=/opt/bin/etcdctl mode=0755
# ����������etcd.service �ļ�
- name: wirte etcd service
  template: src=etcd.service.j2 dest=/etc/systemd/system/etcd.service

# ����etcd��Ⱥ
- name: daemon reload
  shell: systemctl daemon-reload

- name: start etcd
  service: name=etcd state=restarted enabled=yes
```

```ssh
# ִ��playbook������etcd��Ⱥ
ansible-playbook etcd.yml -i inventory
```
