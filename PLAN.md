В Markdown, который используется в GitHub, нет встроенной поддержки спойлеров как в HTML (<details>), но GitHub поддерживает тег <details> с <summary> для создания сворачиваемых секций. Я обновлю файл, поместив структуру проекта под спойлеры с использованием этой разметки, чтобы она корректно отображалась в GitHub.
Вот исправленная версия:
markdown
# Ansible: Развертывание Nginx-сервера

## Описание
Этот проект демонстрирует использование Ansible для автоматизации развертыванию веб-сервера Nginx. Основной фокус — безопасность, производительность и способность распределять нагрузку между несколькими серверами. Проект подходит для демонстрации навыков DevOps-инженера уровня Middle.

## Цели
- Установить Nginx на одном или нескольких узлах.
- Настроить безопасную и оптимизированную конфигурацию.
- Обеспечить балансировку нагрузки для кластера серверов.
- Ускорить процесс развертывания через параллелизм и кэширование.

## Структура проекта
<details>
<summary>Посмотреть структуру</summary>
ansible/
├── ansible.cfg
├── deploy_nginx.yml
├── inventory/
│   └── hosts.yml
├── roles/
│   ├── nginx/
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   ├── templates/
│   │   │   └── nginx.conf.j2
│   │   └── vars/
│   │       └── main.yml
└── README.md

</details>

## Требования
- Ansible 2.9 или выше
- SSH-доступ к целевым серверам
- Права sudo на серверах

## Основной код

### 1. Inventory (`inventory/hosts.yml`)
Список серверов для развертывания.
```yaml
all:
  children:
    webservers:
      hosts:
        web1:
          ansible_host: 192.168.1.10
          ansible_user: ubuntu
          ansible_ssh_private_key_file: ~/.ssh/id_rsa
        web2:
          ansible_host: 192.168.1.11
          ansible_user: ubuntu
          ansible_ssh_private_key_file: ~/.ssh/id_rsa
2. Playbook (deploy_nginx.yml)
Запускает роль на всех узлах с оптимизацией.
yaml
- name: Deploy Nginx with Ansible
  hosts: webservers
  strategy: free
  gather_facts: yes
  roles:
    - nginx
3. Роль Nginx
Задачи (roles/nginx/tasks/main.yml)
Установка, настройка и проверка серверов.
yaml
- name: Check backend servers availability
  ansible.builtin.wait_for:
    host: "{{ item }}"
    port: 80
    timeout: 10
  loop: "{{ groups['webservers'] | map('extract', hostvars, 'ansible_host') | list }}"
  ignore_errors: yes

- name: Ensure Nginx is installed
  ansible.builtin.package:
    name: nginx
    state: present
  become: yes

- name: Copy Nginx configuration file
  ansible.builtin.template:
    src: nginx.conf.j2
    dest: /etc/nginx/nginx.conf
    mode: '0644'
    owner: root
    group: root
  become: yes
  notify: Restart Nginx

- name: Ensure Nginx is running and enabled
  ansible.builtin.service:
    name: nginx
    state: started
    enabled: yes
  become: yes

- name: Restart Nginx
  ansible.builtin.service:
    name: nginx
    state: restarted
  become: yes
Шаблон конфигурации (roles/nginx/templates/nginx.conf.j2)
Безопасная и оптимизированная конфигурация с балансировкой.
nginx
user www-data;
worker_processes auto;
pid /run/nginx.pid;

events {
    worker_connections 1024;
}

http {
    server_tokens off;
    limit_req_zone $binary_remote_addr zone=req_limit:10m rate=10r/s;
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml;
    gzip_min_length 256;

    upstream backend {
        server 192.168.1.10;
        server 192.168.1.11;
    }

    server {
        listen {{ nginx_port }};
        server_name {{ server_name | default('localhost') }};

        add_header X-Frame-Options "DENY";
        add_header X-Content-Type-Options "nosniff";
        add_header X-XSS-Protection "1; mode=block";

        location / {
            proxy_pass http://backend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            limit_req zone=req_limit burst=20;
        }

        location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
            expires 30d;
        }
    }
}
Переменные (roles/nginx/vars/main.yml)
Гибкие настройки.
yaml
nginx_port: 80
server_name: "example.com"
4. Оптимизация Ansible (ansible.cfg)
Ускорение выполнения.
ini
[defaults]
forks = 10
fact_caching = jsonfile
fact_caching_timeout = 86400
Улучшения
Безопасность
Ограничение прав с become: yes только для нужных задач.
Заголовки безопасности и лимит запросов в Nginx.
Рекомендация отключить пароли для SSH.
Скорость
Gzip и кэширование статических файлов в Nginx.
Параллельное выполнение (strategy: free) и кэширование фактов в Ansible.
Распределение нагрузки
Балансировка через upstream в Nginx.
Проверка доступности бэкендов перед развертыванием.
Инструкции по запуску
Настрой SSH-доступ к серверам:
Сгенерируй ключ:
bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
Скопируй на сервер:
bash
ssh-copy-id ubuntu@192.168.1.10
Отключи пароли в /etc/ssh/sshd_config:
bash
PasswordAuthentication no
sudo systemctl restart sshd
Обнови inventory/hosts.yml с IP твоих серверов.
Запусти playbook:
bash
ansible-playbook -i inventory/hosts.yml deploy_nginx.yml
Проверь работу:
bash
curl http://192.168.1.10
Почему это круто?
Безопасность: Защита от DDoS, XSS и утечек информации.
Скорость: Быстрое развертывание и отклик сервера.
Масштабируемость: Поддержка кластера серверов.

---

### Что изменилось
- **Спойлеры**: Структура проекта теперь под `<details><summary>`, что делает её сворачиваемой в GitHub. При клике на "Посмотреть структуру" откроется дерево файлов.
- **Проверка**: Всё остальное осталось без изменений, разметка сохраняет корректность (заголовки, списки, блоки кода).
- **Отображение**: В GitHub структура будет скрыта по умолчанию, что улучшит читаемость, а код останется легко копируемым.

Сохрани этот текст как `ansible/README.md` в твоём репозитории, и он будет выглядеть аккуратно. Если нужно что-то ещё подправить, дай знать!
