![image](https://user-images.githubusercontent.com/25949605/230129141-f5ad1771-bd40-4531-af0c-062970905148.png)
Использованные команды:
1. Установка postgressql

sudo apt install postgresql

2. Добавления репозитория zabbix

wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-
release_6.0-4%2Bdebian11_all.deb
dpkg -i zabbix-release_6.0-4+debian11_all.deb
apt update

3. Установка Zabbix сервер, веб-интерфейс

sudo apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts nano -y

4. Создание пользователя с помощью psql из-под root

su - postgres -c 'psql --command "CREATE USER zabbix WITH PASSWORD
'\'123456789\'';"'
su - postgres -c 'psql --command "CREATE DATABASE zabbix OWNER zabbix;"'

5. Импорт схемы

zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix

6. Задаём пароль в DBPassword

sed -i 's/# DBPassword=/DBPassword=123456789/g' /etc/zabbix/zabbix_server.conf

7. Запускаем сервер

sudo systemctl restart zabbix-server apache2 
sudo systemctl enable zabbix-server apache2 
![image](https://user-images.githubusercontent.com/25949605/230129277-9ea81d61-36e2-4dce-a563-e22ec7707ff8.png)
![image](https://user-images.githubusercontent.com/25949605/230129299-722b64ce-33ce-40d9-ba9b-38d5e0338a16.png)
Установка Zabbix-agent:

wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/

zabbix-release_6.0-4%2Bdebian11_all.deb

dpkg -i zabbix-release_6.0-4+debian11_all.deb

apt update

sudo apt install zabbix-agent -y

sudo systemctl restart zabbix-agent

sudo systemctl enable zabbix-agent

Просмотр логов взаимодействия агента и сервера:

cat /var/log/zabbix/zabbix_agentd.log
![image](https://user-images.githubusercontent.com/25949605/230129363-dae7b296-6ef2-44d9-89a8-49a180ea9fb6.png)
![image](https://user-images.githubusercontent.com/25949605/230129390-65e7b210-2f2c-423d-9f3e-11a45b517d9c.png)
![image](https://user-images.githubusercontent.com/25949605/230129425-42cfa8b9-1879-464e-9851-830e7b3508f3.png)
