# DevOps Portfolio

Привет! Меня зовут Меня зовут Сергей, и это мое портфолио. Здесь я демонстрирую свои навыки работы с современными инструментами и автоматизацией процессов.

## 🔧 Навыки
- **Инструменты**: Ansible, Grafana, Prometheus, Docker, Docker Compose, Docker Swarm, Terraform, Kubernetes, Nginx
- **Языки**: Python (Flask, Django, docker-py), Bash, PowerShell
- **Автоматизация**: CI/CD, мониторинг, управление инфраструктурой

## 📂 Структура репозитория
```
DevOps-Portfolio/
├── README.md
├── ansible/        # Автоматизация инфраструктуры
├── monitoring/     # Мониторинг с Grafana и Prometheus
├── docker/        # Контейнеризация приложений
├── terraform/     # Инфраструктура как код
├── kubernetes/    # Оркестрация контейнеров
├── nginx/         # Конфигурация веб-серверов
├── automation/    # Скрипты на Python, Bash, PowerShell
└── docs/          # Документация и скриншоты
```

## 🚀 Проекты

### 1. Ansible: Автоматизация развертывания инфраструктуры
**Идея:** Написание playbook для развертывания Nginx на нескольких узлах.
#### 🔹 Что демонстрируется:
- Установка и настройка Nginx через Ansible
- Конфигурация балансировки нагрузки
- Использование ролей и переменных

#### 📂 Пример структуры
```
ansible/
├── deploy_nginx.yml
├── roles/
│   ├── nginx/
│   │   ├── tasks/
│   │   ├── templates/
│   │   └── vars/
└── inventory/
```

### 2. Monitoring: Grafana + Prometheus
**Идея:** Мониторинг Nginx с Prometheus и визуализацией в Grafana.
#### 🔹 Что демонстрируется:
- Конфигурация Prometheus для сбора метрик через nginx-exporter
- Dashboard в Grafana с запросами, нагрузкой и ошибками
- Запуск компонентов через Docker Compose

#### 📂 Пример структуры
```
monitoring/
├── prometheus.yml
├── docker-compose.yml
└── grafana/
    └── dashboards/
        └── nginx_metrics.json
```

### 3. Docker & Docker Compose: Контейнеризация приложения
**Идея:** Создание простого веб-приложения (Flask) в Docker.
#### 🔹 Что демонстрируется:
- Dockerfile для сборки
- Docker Compose для запуска с Nginx как reverse proxy

#### 📂 Пример структуры
```
docker/
├── Dockerfile
├── docker-compose.yml
├── app/
│   └── app.py
└── nginx/
    └── nginx.conf
```

### 4. Docker Swarm: Оркестрация контейнеров
**Идея:** Развертывание приложения в кластере Docker Swarm.
#### 🔹 Что демонстрируется:
- Настройка Swarm-кластера
- Деплой сервисов (app + Nginx) с масштабированием

#### 📂 Пример структуры
```
docker/swarm/
├── docker-stack.yml
└── README.md
```

### 5. Terraform: Инфраструктура как код
**Идея:** Развертывание виртуальной машины в облаке (AWS EC2) с Docker.
#### 🔹 Что демонстрируется:
- Создание EC2-инстанса
- Установка Docker через user_data

#### 📂 Пример структуры
```
terraform/
├── main.tf
├── variables.tf
└── outputs.tf
```

### 6. Kubernetes: Оркестрация кластера
**Идея:** Развертывание Flask-приложения в Kubernetes с ingress-контроллером.
#### 🔹 Что демонстрируется:
- Манифесты для Pod, Deployment, Service
- Настройка ingress с Nginx

#### 📂 Пример структуры
```
kubernetes/
├── deployment.yml
├── service.yml
├── ingress.yml
└── README.md
```

### 7. Nginx: Балансировка нагрузки
**Идея:** Использование Nginx как балансировщика нагрузки.
#### 🔹 Что демонстрируется:
- Конфигурация upstream и proxy

#### 📂 Пример структуры
```
nginx/
├── nginx.conf
└── README.md
```

### 8. Automation: Скрипты на Python, Bash и PowerShell
**Идея:** Автоматизация DevOps-задач.
#### 🔹 Что демонстрируется:
- Python: Скрипт проверки статуса Docker-контейнеров
- Bash: Резервное копирование конфигурации Nginx
- PowerShell: Мониторинг ресурсов Windows-сервера

#### 📂 Пример структуры
```
automation/
├── python/
│   └── docker_cleanup.py
├── bash/
│   └── nginx_backup.sh
└── powershell/
    └── server_monitor.ps1
```

## 💡 Советы по реализации
- **Интеграция инструментов**: Используйте Ansible для настройки Prometheus, Terraform для создания Kubernetes-кластера.
- **Документация**: В каждом проекте добавьте README с инструкциями.
- **Тестирование**: Убедитесь, что всё работает локально или в облаке (AWS Free Tier).
- **CI/CD**: Добавьте GitHub Actions для автоматизации тестов или деплоя.

## 🎯 Как использовать
Инструкции для каждого проекта находятся в соответствующих папках. 🚀
