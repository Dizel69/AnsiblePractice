# Проект по автоматизации с использованием Ansible

## Описание

Этот проект направлен на автоматизацию настройки серверов с использованием **Ansible**. В процессе были рассмотрены следующие аспекты:

1. Управление конфигурациями с помощью **Ansible Playbooks**.
2. Использование **ролевой структуры** для организации задач.
3. Применение **шаблонов Jinja2** для гибкой настройки конфигурационных файлов.
4. Настройка окружений (например, **staging** и **production**) с помощью переменных

---

## Структура проекта

```
├── AnsiblePractice/                       # Корневая директория проекта
    ├── README.md                          # Описание проекта
    ├── ansible.cfg                        # Основной конфиг для настройки Ansible 
    ├── playbook.yml                       # Главный playbook для запуска задач
    ├── inventory/                         # Папка с инвентарями для разных окружений
    |   ├── production.ini                 # Инвентарь для production окружения 
    |   └── staging.ini                    # Инвентарь для staging окружения
    └── roles/                             # Директория с ролями для выполнения задач
        ├── nginx/                         # Роль для настройки Nginx
        │   ├── tasks/                     # Задачи, связанные с Nginx
        │   │   └── main.yml               # Основной файл задач для Nginx 
        │   ├── handlers/                  # Обработчики, такие как перезапуск Nginx 
        │   |   └── main.yml               # Обработчик для перезапуска Nginx
        |   ├── templates/                 # Шаблоны для конфигурации
        |   |   └── nginx.conf.j2          # Шаблон конфигурации Nginx 
        |   └── vars/                      # Переменные для Nginx
        |       ├── main.yml               # Основные переменные 
        |       ├── staging.yml            # Переменные для staging окружения
        |       └── production.yml         # Переменные для production окружения
        ├── user_management/               # Роль для управления пользователями
        │   └── tasks/                     # Задачи для работы с пользователями
        │       └── main.yml               # Основной файл задач для пользователей
        ├── ufw/                           # Роль для настройки фаервола
        |   └── tasks/                     # Задачи для настройки фаервола
        |       └── main.yml               # Основной файл задач для фаервола
        └── python_setup/                  # Роль для настройки Python и Flask
            ├──templates/                  # Шаблоны для конфигурации Python приложений 
            |  └── flask_app_config.py.j2  # Шаблон конфигурации Flask
            ├── vars/                      # Переменные для Python и Flask
            |   └── main.yml               # Основные переменные для Python
            └── tasks/                     # Задачи для установки Python и зависимостей 
                └── main.yml               # Основной файл задач для Python

```

## Этапы разработки

### 1. **Настройка окружений в Ansible**

Были созданы два окружения:

- **staging** (для тестирования).
- **production** (для продакшена).

Для каждого окружения настроены разные конфигурации и параметры. Это позволяет нам изолировать настройки для каждого окружения и легко изменять их, когда это необходимо.

### 2. **Шаблоны в Ansible**

Были использованы **шаблоны Jinja2** для генерации конфигурационных файлов. Например, для конфигурации Nginx создаётся шаблон, который в зависимости от окружения (staging или production) генерирует разные настройки для сервера. Это позволяет нам:

- Легко менять конфигурации.
- Поддерживать разные параметры для разных окружений, не меняя основной код.

Пример шаблона: конфигурация для Nginx, которая автоматически подставляет нужные значения в зависимости от окружения.

### 3. **Роли в Ansible**

Для улучшения структуры и организации кода были использованы **роли**. Роль в Ansible — это набор задач, переменных и файлов, которые выполняются для настройки конкретной части системы. Я создал следующие роли:

- **nginx**: для установки и настройки Nginx.
- **python_setup**: для установки Python и необходимых зависимостей.
- **user_management**: для управления пользователями и правами доступа на сервере.
- **ufw**: для настройки фаервола и ограничения доступа к серверам.

Роли упрощают масштабируемость и повторное использование кода, так как ты можешь их использовать для различных проектов и окружений.

### 4. **Переменные и конфигурация**

Были применены переменные в Ansible для динамической настройки конфигураций в зависимости от окружения. Например, в файле `staging.yml` мы указываем параметры для окружения staging, а в `production.yml` — для продакшн. Эти переменные затем подставляются в шаблоны конфигурации и другие части кода.

### 5. **Настройка фаервола (UFW)**

Так же настроен **UFW (Uncomplicated Firewall)** с помощью Ansible для блокировки несанкционированного доступа к серверам. Конкретные задачи:

- Разрешение портов для HTTP (80) и HTTPS (443).
- Блокировка всех остальных портов, кроме SSH (порт 22).
- Включение фаервола для повышения безопасности.

Конфигурация фаервола управляется ролью **ufw**, в которой описаны все необходимые задачи.

### 6. **Обработчики в Ansible**

Были использованы обработчики для перезапуска сервисов, если конфигурационные файлы были изменены. Например, если мы изменяем конфигурацию Nginx, обработчик перезапускает Nginx, чтобы изменения вступили в силу.

---

## Заключение

Я использовал Ansible для автоматизации настройки серверов и приложений, а также для организации гибкости через различные конфигурации и шаблоны. Это позволяет быстро и удобно управлять настройками разных окружений (staging, production) с минимальными изменениями в коде.

---

## TODO

1. Добавить поддержку новых сервисов (например, базы данных или очередь сообщений).
2. Улучшить работу с переменными и шаблонами для ещё большей гибкости.
3. Работать над оптимизацией процессов, например, настроить параллельное выполнение задач в Ansible для улучшения производительности.
