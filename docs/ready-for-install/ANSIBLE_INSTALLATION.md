# Ansible Installation


ansible���³��ֵ��Զ�����ά���ߣ�����Python�������������ڶ���ά���ߣ�puppet��cfengine��chef��func��fabric�����ŵ㣬ʵ��������ϵͳ���á�����������������������ȹ��ܡ�
ansible�ǻ���ģ�鹤���ģ�����û������������������������������������ansible�����е�ģ�飬ansibleֻ���ṩһ�ֿ�ܡ���Ҫ������
- �Ӳ��connection plugins������ͱ���ض�ʵ��ͨ�ţ�
- ost inventory��ָ����������������һ�������ļ����涨���ص�������
- ����ģ�����ģ�顢commandģ�顢�Զ���ģ�飻
- �����ڲ����ɼ�¼��־�ʼ��ȹ��ܣ�
- playbook���籾ִ�ж������ʱ���Ǳ�������ýڵ�һ�������ж������ 

��ϸ���Բο���http://www.ansible.com.cn/docs/intro.html

### ��װansible�Թ���������Ҫ��
- ��װ��Python 2.6 �� Python 2.7 (windowsϵͳ����������������),����������Ansible.���ﰲװ����python2.7
- ������ϵͳ������ Red Hat, Debian, CentOS, OS X, BSD�ĸ��ְ汾,����ֻ�õ���centos7

### ��װansible���йܽڵ��Ҫ��
- ��װpython2.6����Python2.7
- ϵͳҪ��ͬ��
- �ܹ�sshͨ��

### ��װansible

- ��Դ�밲װ

��װansible֮ǰ�����µ�Pythonģ��Ҳ��Ҫ��װ
```sh
$ sudo pip install paramiko PyYAML Jinja2 httplib2 six
```
��Ҳ����ֱ�Ӱ�װ��Щ��������
```sh
$ cd  packages
$ rpm -ivh *.rpm
```
��װansible
```sh
$ git clone git://github.com/ansible/ansible.git --recursive
# or get the ansibel packages from directory "packages"
$ tar -xvzf  ansible.tar.gz
$ cd ./ansible
$ python setup.py install
```

### ͨ��Yum��װ���·����汾
ͨ��Yum��װRPMs������ EPEL 6, 7, �Լ�����֧���е�Fedora���а�.

�йܽڵ�Ĳ���ϵͳ�汾�����Ǹ���İ汾(�� EL5), �����밲װ Python 2.4 ����߰汾��Python.

Fedora �û���ֱ�Ӱ�װAnsible, ��RHEL��CentOS�û�,��Ҫ ���� EPEL

����epelԴ��
```sh
$ wget http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm
$ rpm -ivh epel-release-7-5.noarch.rpm
```
��װansible
```sh
# install the epel-release RPM if needed on CentOS, RHEL, or Scientific Linux
$ sudo yum install ansible
```
�ο���Դ��http://www.ansible.com.cn/docs/intro_installation.html



