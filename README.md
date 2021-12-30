# Jenkins_Installation_Redhat_8

---
- name: Install jenkins
  hosts: jenkins
  become: true
  gather_facts: no

  tasks:

  - name: uninstall java
    yum:
      name: java
      state: removed
  - name: install java
    yum:
      name:  java-11-openjdk-devel
      state: installed

  - name: install wget
    yum:
      name:  wget
      state: installed

  - name: downloading jenkins repo
    get_url:
       url: https://pkg.jenkins.io/redhat-stable/jenkins.repo
       dest: /etc/yum.repos.d/jenkins.repo

  - name: adding jenkins key
    rpm_key:
      state: present
      key: https://pkg.jenkins.io/redhat-stable/jenkins.io.key

  - rpm_key:
      state: present
      key: https://rpms.remirepo.net/RPM-GPG-KEY-remi2018

  - rpm_key:
      state: present
      key: https://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-8

  - name: Install epel repo
    yum:
      name: https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
      state: present

  - name: yum install jenkins
    yum:
      name: jenkins
      state: installed

  - name: start jenkins services
    service:
      name: jenkins
      state: started
      enabled: yes
