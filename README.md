ansible-role-jira
=========
Роль для установки/настройки сервиса JIRA.

Требования
------------

- Systemd на целевом хосте
- Firewalld

Переменные 
--------------

| Variable | Type | Default | Comment |
| ------ | ------ | -------  | ---------- |
| victoriametrics_version | string | v1.44.0 | Версия |
| jira_user | string | jira | Пользователь JIRA |
| jira_group | string | jira | Группа пользователя JIRA |
| jira_home | string | /var/atlassian/jira | Домашняя директория сервиса JIRA |
| jira_download_dir | string | /tmp/jiradownload | Директория загрузки архива |
| jira_installation_dir | string | /opt/atlassian/jira | Директория установки |
| jira_attachments_dir | string | "{{ jira_home }}/data/attachments" | Директория для вложений |
| jira_version | string | 8.10.0 | Версия JIRA для установки |
| jira_jvm_support_recommended_args | string | "-Datlassian.plugins.enable.wait=300" | Доп. параметры |
| jira_required_args | string | "-Djava.awt.headless=true -Datlassian.standalone=JIRA -Dorg.apache.jasper.runtime.BodyContentImpl.LIMIT_BUFFER=true -Dmail.mime.decodeparameters=true -Dorg.dom4j.factory=com.atlassian.core.xml.InterningDocumentFactory" | Параметры запуска |
| jira_jmx_enabled | bool | false | JMX-мониторинг |
| jira_jmx_port | int | 8090 | Порт для мониторинга |
| jira_disable_notifications | bool | true | Отключить уведомления по E-mail  |
| jira_jvm_minimum_memory | string | 2500m | JVM minimum memory |
| jira_jvm_maximum_memory | string | 4096m | JVM maximum memory |
| jira_server_port | int | 8005 | Порт сервера Tomcat |
| jira_connector_port | int | 8080 | Порт JIRA |
| jira_proxy_name | string | "" | Адрес прокси |
| jira_context_path | string | "" | Jira Context path |
| jira_database_configuration | bool | true | Выполнять конфигурацию БД |
| jira_database_type | string | postgres72 | Тип БД |
| jira_database_driver_class | string | org.postgresql.Driver | Используемый драйвер подключения к БД |
| jira_database_username | string | atlassian | Имя пользователя БД |
| jira_database_password | string | "" | Пароль пользователя БД |
| jira_database_host | string | localhost | Хост подключения к БД |
| jira_database_port | int | 5432 | Порт подключения к БД |
| jira_database_name | string | jira | Имя БД |
| jira_database_params | string | ssl=true | Доп. параметры подключения к БД |
| jira_install_additional_actions | string | "" | Дополнительные 
| jira_install_datacenter | bool | false | Сконфигурировать узел для редакции Data Center |  
| jira_datacenter_node_id | string | node-1 | Имя узла |
| jira_datacenter_shared_home | string | /var/jira_shared_home | Общая директория узлов (NFS) |
| jira_service_name | string | jira | Название службы |
| jira_service_state | string | started | Целевое состояние службы после установки |
| jira_service_enabled | string | true | Запускать службу при старте системы |

Пример Плэйбука
----------------

     - hosts:
         - jira
       become: true
       vars:
           jira_database_password: SomePa$$w0rd
           jira_database_port: 6432
           jira_database_host: rc1a-ceey0dj4zien5mhh.mdb.yandexcloud.net
           jira_install_datacenter: true
           jira_install_additional_actions: >-
               mkdir -p ~/.postgresql &&
               wget --no-check-certificate "https://storage.yandexcloud.net/cloud-certs/CA.pem" -O ~/.postgresql/root.crt &&
               chmod 0600 ~/.postgresql/root.crt
           openjdk_version: 1.8.0
       roles:
         - ansible-role-openjdk
         - ansible-role-jira
