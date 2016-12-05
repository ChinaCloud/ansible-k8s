# ����Kubernetes Master�ڵ�

* ������play����ָ��ִ��task��Ŀ����Ͷ���,master����Ϊmaster������
```ssh
- hosts: masters
  roles:
    - masters
```
* ���������б�

```ssh
# ��master�ڵ��ϣ�������ִ���ļ����·��
- name: create /opt/bin directory
  file: path=/opt/bin state=directory mode=0755

#  
# copy flanneld, kubelet, kube-dns, kubectl��ִ���ļ���Ŀ������·��
# ���б�����k8s_version��flanneld_version����default/main.yml�ļ��������Ͷ���
#
- name: copy flanneld
  copy: src=flanneld-{{ flanneld_version }} dest=/opt/bin/flanneld mode=0755
- name: copy flanneld
  copy: src=kubelet-{{ k8s_version }} dest=/opt/bin/kubelet mode=0755
- name: copy kube-dns
  copy: src=kube-dns-{{ k8s_version }} dest=/opt/bin/kube-dns mode=0755
- name: copy kubectl
  copy: src=kubectl-{{ k8s_version }} dest=/opt/bin/kubectl mode=0755
- name: copy kubectl
  copy: src=kubectl-{{ k8s_version }} dest=/usr/local/bin/kubectl mode=0755
  
# ���kube-proxy�Լ�kube-kubelet 8080�˿��Ƿ�����
- name: copy wupiao
  copy: src=wupiao dest=/opt/bin/wupiao mode=0755
```

```ssh
# ����ģ�壬����service�ļ�
- name: write flanneld.service
  template: src=flanneld.service.j2 dest=/etc/systemd/system/flanneld.service
- name: write docker.service
  template: src=docker.service.j2 dest=/etc/systemd/system/docker.service
- name: write kube-kubelet.service file
  template: src=kube-kubelet.service.j2 dest=/etc/systemd/system/kube-kubelet.service
- name: write kube-dns.service file
  template: src=kube-dns.service.j2 dest=/etc/systemd/system/kube-dns.service

```

```ssh
# ����yaml�ļ���kubernetes����������yaml�ļ��Զ�����ϵͳ���
- name: create /etc/kubernetes/manifests directory
  file: path=/etc/kubernetes/manifests state=directory mode=0755
- name: write kube-apiserver.yaml file
  template: src=kube-apiserver.yaml.j2 dest=/etc/kubernetes/manifests/kube-apiserver.yaml
- name: write kube-proxy.yaml file
  template: src=kube-proxy.yaml.j2 dest=/etc/kubernetes/manifests/kube-proxy.yaml
- name: write kube-scheduler.yaml file
  template: src=kube-scheduler.yaml.j2 dest=/etc/kubernetes/manifests/kube-scheduler.yaml
- name: write kube-controller-manager.yaml file
  template: src=kube-controller-manager.yaml.j2 dest=/etc/kubernetes/manifests/kube-controller-manager.yaml
```
```ssh
# ���¼���service�ļ�������������
- name: daemon reload
  shell: systemctl daemon-reload
- name: start flanneld
  service: name=flanneld state=restarted enabled=yes
- name: start docker
  service: name=docker state=restarted enabled=yes
- name: start kube-kubelet
  service: name=kube-kubelet state=restarted enabled=yes
- name: start kube-dns
  service: name=kube-dns state=restarted enabled=yes

```

```ssh
# ִ��playbook������master�ڵ�
ansible-playbook master.yml -i inventory
```




