# Установка и настройка Midnight Commander

## Установка вручную

Установка роли выполняется обычным клонированием репозитория

```shell
cd ansible/roles
git clone 'https://github.com/athena210/ansible-role-mc.git' mcconfig
```

## Установка списком

Установку можно выполнять из списка в файле `requirements-galaxy.yml`

```yaml
---
roles:
  - src: https://github.com/athena210/ansible-role-mc.git
    name: mcconfig
    scm: git
    version: master
```

Запуск установки

```shell
cd ansible
ansible-galaxy install -p roles -r requirements-galaxy.yml
```

## Использование роли

Параметры роли
|Переменная|default|Описание|
|-|-|-|
|mcconfig**install_mc|true|Выполнять ли установку MC|
|mcconfig**pkg_cache_update|true|Выполнять ли apt update при установке (часто необходимо на свежей ОС)|
|mcconfig\_\_userdirs|["/root"]|Массив домашних каталогов, куда будут скопированы настроки|

Пример плейбука

```yaml
- name: Global play
  hosts: all

  roles:
    - role: mcconfig
      vars:
        mcconfig_userdirs:
          - /root
          - /home/user
```

Кроме настройки визуальной составляющей MC, также выполняется установка systemd сервиса динамического меню MC.

Сервисы `mc-menu.service` и `mc-menu.path` следят за изменением папки `/etc/mc_menu.d/` и объединяют `*.conf` файлы оттуда в общее меню `/etc/mc/mc.menu`.
