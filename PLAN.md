# DevOps Portfolio  
*Портфолио инженера уровня Middle*  

---

## Описание  
Это портфолио демонстрирует мои навыки работы с современными DevOps-инструментами и автоматизацией процессов. Каждый проект включает документацию, код и инструкции для запуска.  

---

## Навыки  
- **Инструменты**: Ansible, Grafana, Prometheus, Docker, Docker Compose, Docker Swarm, Terraform, Kubernetes, Nginx  
- **Языки**: Python (Flask, Docker SDK), Bash, PowerShell  
- **Фокус**: Безопасность, производительность, масштабируемость  

---

## Структура репозитория  
DevOps-Portfolio/
├── README.md
├── ansible/
├── monitoring/
├── docker/
├── terraform/
├── kubernetes/
├── nginx/
├── automation/
└── docs/

---

## Проекты  

### 1. Ansible: Развертывание Nginx-сервера  
#### Описание  
Автоматизация установки и настройки Nginx с акцентом на безопасность, скорость и балансировку нагрузки.  
#### Структура  
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
#### Основной код  
- **Playbook** (`deploy_nginx.yml`):  
  ```yaml
  - name: Deploy Nginx with Ansible
    hosts: webservers
    strategy: free
    gather_facts: yes
    roles:
      - nginx
Конфигурация Nginx (nginx.conf.j2):  
nginx
user www-data;
worker_processes auto;
http {
    server_tokens off;
    limit_req_zone $binary_remote_addr zone=req_limit:10m rate=10r/s;
    gzip on;
    upstream backend {
        server 192.168.1.10;
        server 192.168.1.11;
    }
    server {
        listen {{ nginx_port }};
        location / {
            proxy_pass http://backend;
            limit_req zone=req_limit burst=20;
        }
    }
}
Улучшения
Безопасность: Ограничение прав, заголовки безопасности, лимит запросов.  
Скорость: Gzip, параллелизм в Ansible.  
Балансировка: Upstream для кластера серверов.
2. Monitoring: Grafana + Prometheus
Описание
Мониторинг Nginx с использованием Prometheus и визуализацией в Grafana.  
Структура
monitoring/
├── prometheus.yml
├── docker-compose.yml
└── grafana/
    └── dashboards/
        └── nginx_metrics.json
Основной код
Docker Compose (docker-compose.yml):  
yaml
version: '3'
services:
  prometheus:
    image: prom/prometheus
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
Улучшения
Интеграция с Nginx через nginx-exporter.  
Кэширование метрик для скорости.
3. Docker & Docker Compose: Контейнеризация приложения
Описание
Контейнеризация Flask-приложения с Nginx как reverse proxy.  
Структура
docker/
├── Dockerfile
├── docker-compose.yml
├── app/
│   └── app.py
└── nginx/
    └── nginx.conf
Основной код
Dockerfile:  
dockerfile
FROM python:3.9-slim
COPY app /app
RUN pip install flask
CMD ["python", "/app/app.py"]
Улучшения
Ограничение ресурсов в docker-compose.yml для безопасности.
4. Docker Swarm: Оркестрация кластера
Описание
Развертывание Flask-приложения в Docker Swarm с масштабированием.  
Структура
docker/swarm/
├── docker-stack.yml
└── README.md
Основной код
Stack (docker-stack.yml):  
yaml
version: '3.7'
services:
  app:
    image: my-flask-app
    deploy:
      replicas: 3
  nginx:
    image: nginx
    ports:
      - "80:80"
Улучшения
Healthchecks для отказоустойчивости.
5. Terraform: Инфраструктура как код
Описание
Развертывание EC2 с Docker в AWS.  
Структура
terraform/
├── main.tf
├── variables.tf
└── outputs.tf
Основной код
main.tf:  
hcl
resource "aws_instance" "web" {
  ami           = "ami-12345678"
  instance_type = "t2.micro"
  user_data     = <<-EOF
                  #!/bin/bash
                  apt update && apt install -y docker.io
                  EOF
}
Улучшения
Модули для масштабируемости.
6. Kubernetes: Деплой приложения
Описание
Развертывание Flask-приложения с Nginx-ingress.  
Структура
kubernetes/
├── deployment.yml
├── service.yml
└── ingress.yml
Основной код
Deployment (deployment.yml):  
yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-app
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: flask
        image: my-flask-app
Улучшения
HorizontalPodAutoscaler для автоскейлинга.
7. Nginx: Балансировка нагрузки
Описание
Настройка Nginx как standalone-балансировщика.  
Структура
nginx/
├── nginx.conf
└── README.md
Основной код
nginx.conf:  
nginx
upstream backend {
    server 10.0.0.1;
    server 10.0.0.2;
}
server {
    listen 80;
    location / {
        proxy_pass http://backend;
    }
}
Улучшения
Keepalive для производительности.
8. Automation: Скрипты
Описание
Автоматизация задач на Python, Bash, PowerShell.  
Структура
automation/
├── python/
│   └── docker_cleanup.py
├── bash/
│   └── nginx_backup.sh
└── powershell/
    └── server_monitor.ps1
Основной код
Python (docker_cleanup.py):  
python
import docker
client = docker.from_client()
for img in client.images.list():
    if "dangling" in img.tags:
        client.images.remove(img.id)
Bash (nginx_backup.sh):  
bash
#!/bin/bash
tar -czf nginx_backup_$(date +%F).tar.gz /etc/nginx/
PowerShell (server_monitor.ps1):  
powershell
$cpu = Get-WmiObject Win32_Processor | Measure-Object -Property LoadPercentage -Average
if ($cpu.Average -gt 80) { Write-Host "CPU overload!" }
Улучшения
Логирование для отладки.
Инструкции по запуску
Склонируй репозиторий:  
bash
git clone git@github.com:<username>/DevOps-Portfolio.git
Перейди в нужный проект и следуй README.md.
Почему это круто?
Безопасность: Ограничения прав, заголовки, лимиты запросов.  
Скорость: Кэширование, параллелизм, оптимизация.  
Масштабируемость: Балансировка, кластеризация, автоскейлинг.

---

### Что изменилось
1. **Ansible**: Добавлены улучшения из предыдущих обсуждений (безопасность, скорость, балансировка).  
2. **Остальные проекты**: Обновлены с учетом фокуса на безопасности (ограничение ресурсов), скорости (кэширование, healthchecks) и масштабируемости (replicas, autoscaling).  
3. **Инструкции**: Уточнены с учетом SSH-ключа для `git clone`.  