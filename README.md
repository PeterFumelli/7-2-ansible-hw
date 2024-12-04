# Домашнее задание к занятию "7.1 Ansible. Часть 2" - Петр Фумелли

### Задание 1

Напишите три плейбука. При написании рекомендуем использовать текстовый редактор с подсветкой синтаксиса YAML.

1. Скачивание, создание папки и распаковка архива

```yaml
---
- name: Download and extract an archive
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

2. Установка tuned и настройка автозапуска

```yaml
---
- name: Install and enable tuned
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

### Задание 2
