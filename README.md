# edu-practice-configurator

# Управление проектами PostgreSQL через Ansible

## Описание логики работы

Данный Ansible-проект предоставляет инструменты для создания и удаления изолированных окружений PostgreSQL для различных проектов. Каждый проект включает:

1. Отдельного пользователя БД
2. Отдельную базу данных
3. Автоматическое назначение прав
4. Безопасное хранение учетных данных

## Структура проекта

```
inventories/
└── pg_projects/          # Конфиги проектов (user, db, password)
    └── {project_name}.yml
roles/
└── pg_project/           # Роль для управления проектами
    └── tasks/
        └── main.yml
playbooks/
├── create_project.yml    # Создание нового проекта
└── drop_project.yml      # Удаление проекта
```

## Основные команды

### Создание нового проекта

```bash
ansible-playbook playbooks/create_project.yml  -e "project_name=timetracker_app"
```

Что происходит при создании:
1. Создается пользователь с именем и праролем, указанными в файле проекта
2. Создается БД с названием указанным в файле проекта
3. Пользователь назначается владельцем созданной БД
4. Учетные данные сохраняются в зашифрованном виде

### Удаление проекта

```bash
ansible-playbook playbooks/drop_project.yml -e "project_name=timetracker_app"
```

Что происходит при удалении:
1. Завершаются все активные подключения к БД
2. Удаляется БД проекта
3. Удаляется пользователь проекта
4. Конфиг проекта перемещается в архив

## Создание конфига для нового проекта

1. Сгенерируйте зашифрованные переменные:
```bash
ansible-vault encrypt_string 'db_user' --name 'user'
ansible-vault encrypt_string 'db_name' --name 'db'
ansible-vault encrypt_string 'secure_password' --name 'password'
```

2. Создайте файл проекта:
```yaml
# inventories/pg_projects/new_project.yml
---
user: !vault |
      $ANSIBLE_VAULT;1.1;AES256
      ...
db: !vault |
      $ANSIBLE_VAULT;1.1;AES256
      ...
password: !vault |
      $ANSIBLE_VAULT;1.1;AES256
      ...
```

## Безопасность

1. Все пароли хранятся в зашифрованном виде
2. Используется отдельный пользователь для каждого проекта
3. При удалении проекта все данные безвозвратно удаляются
4. Конфиги удаленных проектов архивируются

## Пример рабочего процесса

1. Создать новый проект:
```bash
ansible-playbook playbooks/create_project.yml -e "project_name=analytics"
```

2. Использовать в приложении:
```python
# Пример подключения
DB_URL = "postgresql://analytics_user:secure_password@localhost/analytics_db"
```

3. Удалить проект после завершения работ:
```bash
ansible-playbook playbooks/drop_project.yml -e "project_name=analytics"
```



## Полезные комнды ansible

### Проверка связи с сервером

Для проверки соединения с сервером используйте команду:

```sh
ansible all -m ping -i inventories/hosts --vault-password-file .vault_pass
```

### Запуск playbook

Для запуска playbook выполните:

```sh
ansible-playbook -i inventories/hosts playbook.yml --vault-password-file .vault_pass
```
### Расшифровка файла, зашифрованного Ansible Vault

Чтобы расшифровать файл, зашифрованный с помощью Ansible Vault, используйте команду:

```sh
ansible-vault decrypt путь/к/файлу --vault-password-file .vault_pass
```

После выполнения этой команды файл будет расшифрован и доступен в открытом виде. Убедитесь, что файл `.vault_pass` содержит правильный пароль и хранится в безопасном месте.

Для вывода значения переменной с помощью Ansible на локальной машине используйте следующую команду:

```sh
ansible localhost -m debug -a "var=имя_переменной" -e "@путь/к/файлу.yml"
```