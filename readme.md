
# WWW SQL Designer

* https://ondras.zarovi.cz/sql/demo/
(Lab4)

# drawSQL

* https://drawsql.app

# DBdesigner

* https://app.dbdesigner.net/


## Импорт базы в Postgres
psql -U postgres postgres < dump.sql

## Экспорт базы из Postgres
pg_dump -U postgres postgres > dump.sql

## Установка Midnight Commander (опционально)
sudo apt install mc
sudo mc

## Ошибки

Peer authentication failed for user "postgres":

* отредактировать /etc/postgresql/13/main/pg_hba.conf, заменить первый "peer" на "trust".

sudo service postgresql stop
sudo service postgresql start
psql -U postgres
ALTER USER postgres WITH PASSWORD 'postgres';
exit;

* отредактировать  /etc/postgresql/13/main/pg_hba.conf, заменить "trust" на "md5".

sudo service postgresql restart

## Установка MySQL (опционально)

sudo apt install mysql-server
sudo mysql -u root

USE mysql;
UPDATE user SET plugin='mysql_native_password' WHERE User='root';
FLUSH PRIVILEGES;
exit;

sudo service mysql restart
sudo apt install phpmyadmin
sudo service apache2 restart

(edit /etc/phpmyadmin/config.inc.php, uncomment $cfg['Servers'][$i]['AllowNoPassword'] = TRUE;)

login to phpmyadmin as: http://localhost/phpmyadmin
login: root
password: (none)
create database test, run test.sql
