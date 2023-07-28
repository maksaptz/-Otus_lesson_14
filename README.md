#  Настройка мониторинга Zabbix

Описание домашнего задания:

В работе использовался vagrant box: "debian/bullseye64" version "11.20230615.1"

В качестве результата прислать скриншот экрана - дашборд должен содержать в названии имя приславшего.

## Цель домашнего задания:

Настроить дашборд с 4-мя графиками:
- память;
- процессор;
- диск;
- сеть.

### Ход выполнения домашнего задания

1) Добавить репозиторий Zabbix;

```wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-1+debian11_all.deb```

```dpkg -i zabbix-release_6.0-1+debian11_all.deb```

3) Установка сервера Zabbix

```apt update -y```

```apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent zabbix-frontend-php zabbix-apache-conf -y```

5) Установить часовой пояс:

#Редактируем файл

```vi /etc/php/7.4/apache2/php.ini```

#Изменяем следующую строку:

date.timezone = UTC

6) Установка и настройка базы данных

#устанавливаем базу данных

```apt install mariadb-server -y```

#заходим в базу

```mysql -u root -p```

7) После входа создайте базу данных и пользователя для Zabbix:

#создаем базу и пользователя

```CREATE DATABASE zabbix character set utf8mb4 collate utf8mb4_bin;```

```GRANT ALL PRIVILEGES ON zabbix.* TO zabbix@localhost IDENTIFIED BY 'password';```

#применяем изменения

```FLUSH PRIVILEGES;```

```EXIT;```

8) Меняем конфиг Zabbix

/etc/zabbix/zabbix_server.conf

#изменяем следующие строки:

DBName=zabbix

DBUser=zabbix

DBPassword=password

#импортируем данные Zabbix:

```zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql -u zabbix -p zabbix```

#Перезагружаем службы

```systemctl restart apache2```

```systemctl enable zabbix-server zabbix-agent apache2```

```systemctl restart zabbix-server zabbix-agent```

#заходим в Zabbix

http://127.0.0.1:8080/zabbix/ - выставляем в зависимости от адреса/порта

9) В web-интерфейсе настраиваем дашборд с требуемыми итемами.

#### Проверка домашнего задания
Смотри screenshot с полученным результатом.
