# Inventory�ļ�
###### Topics
* [��������](#��������)
* [��������](#��������)
* [��ı���](#��ı���)
* [Inventory������˵��](#Inventory������˵��)

### ��������

###### ����

��������һЩ��̬IP��ַ,ϣ������һЩ����,��������ϵͳ�� host �ļ�������,�ֻ�������ͨ�����������,��ô������������:

```sh
# [name]  ansible_host=[IP] ansible=[username] ansible_ssh_pass=[password]
registry ansible_host=172.16.xx.12 ansible_user=root ansible_ssh_pass=centos
dns ansible_host=172.16.xx.13 ansible_user=root
master ansible_host=172.16.80.xx.14 ansible_user=root
minion01 ansible_host=172.16.xx.15 ansible_user=root
minion02 ansible_host=172.16.xx.16 ansible_user=root
```
�����������SSH�˿ڲ��Ǳ�׼��22�˿�,����������֮����϶˿ں�,��ð�ŷָ�.SSH �����ļ����г��Ķ˿ںŲ����� paramiko ������ʹ��,���� openssh ������ʹ��.
```ssh
test01 ansible_ssh_port=5555 ansible_ssh_host=192.168.xx.xx
```
�����������,ͨ�� ��test01�� ����,������ 192.168.xx.xx:5555.��ס,����ͨ�� inventory �ļ������Թ������õı���

##### ��
```ssh
[machines]
master
minion01
minion02
```

```ssh
[etcd]
master
minion01
minion02
```
������[]��������,���ڶ�ϵͳ���з���,���ڶԲ�ͬϵͳ���и���Ĺ���.
һ��ϵͳ�������ڲ�ͬ����,����һ̨����������ͬʱ���� machines �� etcd.��ʱ����������ı���������Ϊ��̨��������

### ��������

ǰ���Ѿ��ᵽ��,�����������������������,��Щ������������ playbooks ��ʹ��:
```ssh
master ansible_host=172.16.80.xx.14 ansible_user=root
minion01 ansible_host=172.16.xx.15 ansible_user=root
minion02 ansible_host=172.16.xx.16 ansible_user=root
```
### ��ı���
���Զ�������������ı���:
```ssh
[registry]
registry

[registry:vars]
registry_host=172.16.xx.12
registry_port=80
```
### Inventory������˵��

```ssh
ansible_ssh_host
      ��Ҫ���ӵ�Զ��������.������Ҫ�趨�������ı�����ͬ�Ļ�,��ͨ���˱�������.

ansible_ssh_port
      ssh�˿ں�.�������Ĭ�ϵĶ˿ں�,ͨ���˱�������.

ansible_ssh_user
      Ĭ�ϵ� ssh �û���

ansible_ssh_pass
      ssh ����(���ַ�ʽ������ȫ,����ǿ�ҽ���ʹ�� --ask-pass �� SSH ��Կ)

ansible_sudo_pass
      sudo ����(���ַ�ʽ������ȫ,����ǿ�ҽ���ʹ�� --ask-sudo-pass)

ansible_sudo_exe (new in version 1.8)
      sudo ����·��(������1.8�����ϰ汾)

ansible_connection
      ����������������.����:local, ssh ���� paramiko. Ansible 1.2 ��ǰĬ��ʹ�� paramiko.1.2 �Ժ�Ĭ��ʹ�� 'smart','smart' ��ʽ������Ƿ�֧�� ControlPersist, ���ж�'ssh' ��ʽ�Ƿ����.

ansible_ssh_private_key_file
      ssh ʹ�õ�˽Կ�ļ�.�������ж����Կ,���㲻��ʹ�� SSH ��������.

ansible_shell_type
      Ŀ��ϵͳ��shell����.Ĭ�������,�����ִ��ʹ�� 'sh' �﷨,������Ϊ 'csh' �� 'fish'.

ansible_python_interpreter
      Ŀ�������� python ·��.�����ڵ����: ϵͳ���ж�� Python, ��������·������"/usr/bin/python",����  \*BSD, ���� /usr/bin/python
      ���� 2.X �汾�� Python.���ǲ�ʹ�� "/usr/bin/env" ����,��Ϊ��Ҫ��Զ���û���·��������ȷ,��Ҫ�� "python" ��ִ�г���������Ϊ python���������(ʵ���п�����Ϊpython26).

      �� ansible_python_interpreter �Ĺ�����ʽ��ͬ,���趨�� ruby �� perl ��·��....
```