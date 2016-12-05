# ��װregistry
֮ǰ�����Ѿ��򵥽��ܹ�playbook,��inventory���÷�������׸������������ֱ�ӽ���registry�Ĵ������̣�
* ���ȴ���play��ָ��ִ��task��Ŀ����Ͷ���
```ssh
---
- hosts: registry
  roles:
    - registry
```

* ����task�б�
```ssh
---
- fail: msg="Bailing out. this play requires 'registry_host'"
  when: registry_host is not defined
- fail: msg="Bailing out. this play requires 'registry_port'"
  when: registry_port is not defined
```
> ����ʹ�����������when������������ִ����䣬�磺fail

```ssh
- name: cp docker service
  template: src=docker.service.j2 dest=/etc/systemd/system/docker.service
- name: daemon reload
  shell: systemctl daemon-reload
- name: start docker.service
  service: name=docker state=started enabled=yes
```
> ������Ҫ����дdocker.service������insecure-registry, Ȼ������docker


```ssh
- name: upload registry image
  copy: src=registry-2.tar dest=/tmp/registry-2.tar

- name: upload pause image
  copy: src=gcr.io~google_containers~pause-amd64~3.0.tar dest=/tmp/gcr.io~google_containers~pause-amd64~3.0.tar
- name: upload hyperkube image
  copy: src=gcr.io-google_containers-hyperkube-v1.3.7.tar dest=/tmp/gcr.io-google_containers-hyperkube-v1.3.7.tar
- name: load registry image
  shell: docker load -i /tmp/registry-2.tar
- name: load pause image
  shell: docker load -i /tmp/gcr.io~google_containers~pause-amd64~3.0.tar
- name: load hyperkube image
  shell: docker load -i /tmp/gcr.io-google_containers-hyperkube-v1.3.7.tar
```
> �ϴ������ر�Ҫ�ľ�����registry������kubernetes���������������pause, hyperkube����

```ssh
- name: copy pip-7.1.2.tar.gz
  copy: src=pip-7.1.2.tar.gz dest=/tmp/pip-7.1.2.tar.gz
- name: install pip
  shell: easy_install /tmp/pip-7.1.2.tar.gz
- name: copy docker-py
  copy: src=docker-py dest=/tmp/
- name: install docker-py
  shell: pip install -U --no-index --find-links=file:///tmp/docker-py docker-py
```
> ��װ������

```ssh
- name: create docker registry
  docker:
  # ���þ�����������
    name: registry
    image: registry:2
  # ��������
    restart_policy: always
    privileged: yes
  # ���ö˿�
    ports:
      - "0.0.0.0:{{ registry_port }}:5000"
    # ��������ӳ�䵽�ⲿ�豸·��
    volumes:
      - "{{ volume_path }}:/var/lib/registry"
    env:
      REGISTRY_DELETE_ENABLED: true
  ignore_errors: true
```
> ����registry������������

```ssh

# ִ��playbook
ansible-playbook registry.yml -i inventory
```

