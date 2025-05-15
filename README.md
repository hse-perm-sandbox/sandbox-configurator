# edu-practice-configurator

## Быстрый старт

### Проверка связи с сервером

Для проверки соединения с сервером используйте команду:

```sh
ansible all -m ping -i inventory/hosts --vault-password-file .vault_pass
```

### Запуск playbook

Для запуска playbook выполните:

```sh
ansible-playbook -i inventory/hosts playbook.yml --vault-password-file .vault_pass
```
### Расшифровка файла, зашифрованного Ansible Vault

Чтобы расшифровать файл, зашифрованный с помощью Ansible Vault, используйте команду:

```sh
ansible-vault decrypt путь/к/файлу --vault-password-file .vault_pass
```

После выполнения этой команды файл будет расшифрован и доступен в открытом виде. Убедитесь, что файл `.vault_pass` содержит правильный пароль и хранится в безопасном месте.
