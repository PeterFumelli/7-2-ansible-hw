# Домашнее задание к занятию "7.1 Ansible. Часть 2" - Петр Фумелли

### Задание 1

Напишите три плейбука. При написании рекомендуем использовать текстовый редактор с подсветкой синтаксиса YAML.

1. Скачивание, создание папки и распаковка архива

```yaml
---
- name: Kafka download&extract
  hosts: all
  become: true
  tasks:
    - name: Create a directory for extraction
      file:
        path: /opt/myarchive
        state: directory

    - name: Download the archive
      get_url:
        url: https://archive.apache.org/dist/kafka/2.8.0/kafka_2.13-2.8.0.tgz
        dest: /tmp/kafka.tgz

    - name: Extract the archive
      unarchive:
        src: /tmp/kafka.tgz
        dest: /opt/myarchive
        remote_src: yes
```

###

KAFKA <https://github.com/PeterFumelli/7-2-ansible-hw/blob/main/img/kafka.png>

2. Установка tuned и настройка автозапуска

```yaml
---
- name: Install&Enable tuned
  hosts: all
  become: true
  tasks:
    - name: Install tuned package
      package:
        name: tuned
        state: present

    - name: Ensure tuned is running
      service:
        name: tuned
        state: started
        enabled: true
```

###

TUNED <https://github.com/PeterFumelli/7-2-ansible-hw/blob/main/img/tuned.png>

3. Изменение motd

```yaml
---
- name: Update MOTD
  hosts: all
  become: true
  vars:
    custom_motd: "Welcome, Admin!"

  tasks:
    - name: Update the motd file
      copy:
        content: "{{ custom_motd }}"
        dest: /etc/motd
```

###

MOTD  <https://github.com/PeterFumelli/7-2-ansible-hw/blob/main/img/motd2.png>

<https://github.com/PeterFumelli/7-2-ansible-hw/blob/main/img/motd.png>

### Задание 2

Модифицируйте плейбук из пункта 3, задания 1. В качестве приветствия он должен установить IP-адрес и hostname управляемого хоста, пожелание хорошего дня системному администратору.

```yaml
---
- name: Update MOTD
  hosts: all
  become: true
  tasks:
    - name: Get the IP address of the host
      command: hostname -I
      register: ip_output

    - name: Get the hostname of the host
      command: hostname
      register: hostname_output

    - name: Update the motd file
      copy:
        content: |
          Welcome to your system!
          Hostname: {{ hostname_output.stdout }}
          IP Address: {{ ip_output.stdout }}
          Отличного дня, Админ!
        dest: /etc/motd
```
