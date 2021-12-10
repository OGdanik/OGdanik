
# Windows

Для windows существуют специальные инсталляторы WAPP (Windows + Apache + Postgres + PHP):

* https://bitnami.com/stack/wapp/installer

Свой index.php можно положить в C:\Bitnami\wappstack-7.4.15-0\apache2\htdocs вместо index.html.

* http://127.0.0.1 (Apache установлен на 80 порт)

В Bitnami используется PHPpgAdmin (если надо, pgAdmin4 можно поставить отдельно).

* http://127.0.0.1/phppgadmin (login postgres/postgres)

Очень мешает кеш PHP, чтобы его отключить надо отредактировать C:\Bitnami\wappstack-7.4.15-0\php\etc\php.ini:

opcache.enable=0

# Linux

## Установка apache2

sudo apt install apache2

## Установка PHP

sudo apt install php php-pgsql
sudo service apache2 restart

* http://localhost (скопировать index.php в /var/www/html)

## Установка Postgres
sudo apt install postgresql postgresql-contrib

## Установка pgadmin4

sudo curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo apt-key add
sudo sh -c 'echo "deb https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list && apt update'
sudo apt install pgadmin4-web
sudo /usr/pgadmin4/bin/setup-web.sh

* http://localhost/pgadmin4 (login/password: postgres/postgres)


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

## Функция возврата практически всей БД

```sql
CREATE FUNCTION list_auto()
    returns table(id integer, vin_nomer character, id_mm integer, marka character, model character, num character, dtv date, 
				  dts date,fio character, dtp date, dtvs date, avariya character, dtavar date)
AS $$

select v.id, v.vin_nomer, v.id_mm, m.marka, m.model, n.number, n.data_vidachi, n.data_snyatiya, vl.fio, vl.data_postanovki,
		vl.data_snyatiya, a.avariya, a.data_avarii
from auto v inner join model_marka m on v.id_mm=m.id 
left join avarii a on a.id_auto_av=v.id
left join reestr_nomerov n on n.id_auto_rn=v.id
left join reestr_vladelcev vl on vl.id_auto_rv=v.id;

$$
language 'sql';
