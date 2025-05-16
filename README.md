# edu-practice-configurator

## Быстрый старт

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