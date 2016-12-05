# playbook

������ansible����Ⱥ֮ǰ���ȼ򵥽���һ��playbook��

����˵,playbooks��һ�ּ򵥵����ù���ϵͳ����������ϵͳ�Ļ���.�����е�����ϵͳ�в�֮ͬ��,�ҷǳ��ʺ��ڸ���Ӧ�õĲ���.
   Playbooks ��������������,��ǿ��ĵط�����,�� playbooks �п��Ա��������ִ�й���,�����������ڶ��������,���������ִ���ر�ָ���Ĳ���.���ҿ���ͬ�����첽�ķ�������.
   playbook�����Ժ�ᵥ����һ���½������ܣ������������Բο���http://www.ansible.com.cn/docs/playbooks_intro.html#about-playbooks
   
Playbooks �ĸ�ʽ��YAML. playbook ��һ������ ��plays�� ���.����������һ���� ��plays�� ΪԪ�ص��б�.�� play ֮��,һ�������ӳ��Ϊ����õĽ�ɫ.

```ssh
# base.yml
----
- hosts: machines
  roles:
    - base
```
> Roles �ĸ����������������뷨��ͨ�� include �����ļ��������������һ����֯��һ����ࡢ�����õĳ���������ַ�ʽ��ʹ�㽫ע��������ط��ڴ���ϣ�ֻ������Ҫʱ��ȥ�����˽�ϸ�ڡ�����rolesΪbase��include������(������task�б��taskִ��ʱ���������ļ�):
```ssh
--|roles
-----base
------files
--------docker-engine-1.12.1-1.el7.centos.x86_64.rpm
------tasks
--------main.yml
```

�ڻ�����ε�Ӧ����,һ��������һ���� ansible ģ��ĵ���. ��plays�� ��������,playbook ������ ��plays�� ���ɵ�����,ͨ�� playbook,���Ա��Ų�����ж�����Ĳ���,������ [machines] ������л���������һ���Ĳ���, Ȼ���� [dns]������һЩ����

��һ�½�[����Inventory�ļ�](./inventory.md)����Ϊplaybook �е�ÿһ�� play��������Ŀ���������,�������ǽ�����Щ�û����ȥ���Ҫִ�еĲ��裨called tasks��.�� ansible ��,play ������,����Ϊ tasks,������.��������Ϊÿһ��play������һ��task�б�һ�� task ��������Ӧ������������ִ�����֮��,��һ�� task �Ż�ִ��.��һ����Ҫ���׵��ǣ�����Ҫ��,��һ�� play ֮��,���� hosts ���ȡ��ͬ������ָ��,���� play ��һ��Ŀ������,Ҳ���ǽ�һ��ѡ���� hosts ӳ�䵽 task, example:
```ssh
$ cat ./roles/base/tasks/main.yml

- name: stop firewalld
  service: name=firewalld state=stopped enabled=no

- name: create /tmp/docker-install directory
  file: path=/tmp/docker-install state=directory mode=0755
  ......
  ....
- name: copy docker-engine-1.12.1-1.el7.centos.x86_64.rpm
  copy: src=docker-engine-1.12.1-1.el7.centos.x86_64.rpm dest=/tmp/docker-install/docker-engine-1.12.1-1.el7.centos.x86_64.rpm
  ......
  ....
- name: install docker engine
  yum: name=/tmp/docker-install/docker-engine-1.12.1-1.el7.centos.x86_64.rpm state=present

```
> ����task �б������Ⱥ�˳����֮ǰ��ָ����Ŀ�����ִ�У���[hosts]������������

���潫��ϸ����task�б��ܣ�

### ͨ��playbook�رշ���ǽ


* ����playbook�еġ�play��, ��base.yml,Ȼ��ָ��������ִ�ж���
---
```ssh
- hosts: machines
  roles:
    - base
```
> ����ָ��machines������ִ�ж���,hosts �е�������һ��������;roles��ָ������������������ִ�е�task��Դ

* ����ִ�еĶ�������task
```ssh
- name: stop firewalld
  service: name=firewalld state=stopped enabled=no
```
>������һ�ֻ����� task �Ķ���,service moudle ʹ�� key=value ��ʽ�Ĳ���,��Ҳ�Ǵ���� module ʹ�õĲ�����ʽ��ÿһ�� task ������һ������ name,���������� playbook ʱ,�������������ִ����Ϣ�п��Ժܺõı�����������һ�� task ��. ���û�ж��� name,��action�� ��ֵ�������������Ϣ�б���ض��� task.

* ִ��playbook ansible_playbook base.yml -i inventory

### ͨ��playbook��װdocker

* �������Ǵ���һ��Ŀ¼�����dockerԴ�ļ�
```ssh
- name: create /tmp/docker-install directory
  file: path=/tmp/docker-install state=directory mode=0755
```
> �����Ǵ���һ������Ϊ/tmp/docker-install��Ŀ¼��Ȩ��Ϊ0755

* �ڶ��������ǽ�����Դ�ļ���ָ��Ŀ¼
```ssh
- name: copy docker-engine-1.12.1-1.el7.centos.x86_64.rpm
  copy: src=docker-engine-1.12.1-1.el7.centos.x86_64.rpm dest=/tmp/docker-install/docker-engine-1.12.1-1.el7.centos.x86_64.rpm
```
> ִ�ж�����copy������Դ��src��Ŀ�ĵأ�dest

* ����������װdocker-engine

```ssh
- name: install docker engine
  yum: name=/tmp/docker-install/docker-engine-1.12.1-1.el7.centos.x86_64.rpm state=present
```
> ִ�ж�����yum install *.rpm

* �����û�
```ssh
- name: add user developer, password is "developer"
  user: name=developer shell=/bin/bash groups=docker password=xxxxxxxxxxxx
```
> �����е�������ǲ�����shell moudule��playbook����ָ������ִ��shell���

```ssh
# ִ��playbook
ansible-playbook base.yml -i inventory
```
