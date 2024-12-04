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

<https://github.com/PeterFumelli/7-2-ansible-hw/blob/main/img/tuned.png>

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


