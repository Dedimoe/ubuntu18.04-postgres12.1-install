# ubuntu18.04-postgres12.1-install
ubuntu18.04-postgres12.1-install

instalation:
```
cd /tmp
wget https://ftp.postgresql.org/pub/source/v12.1/postgresql-12.1.tar.gz
tar -xzvf postgresql-12.1.tar.gz
cd postgresql-12.1


./configure --with-icu --with-openssl --with-systemd --with-libxml --with-libxslt
make
make check
sudo make install
```

postgres database  setting:
```
sudo useradd postgres
sudo passwd postgres

ATA_DIR=/usr/local/pgsql/data
sudo mkdir $DATA_DIR
sudo chown postgres $DATA_DIR

sudo ln -s /usr/local/pgsql/bin/initdb /usr/bin
sudo ln -s /usr/local/pgsql/bin/psql /usr/bin
sudo ln -s /usr/local/pgsql/bin/pg_dump /usr/bin
sudo ln -s /usr/local/pgsql/bin/createdb /usr/bin

su - postgres
initdb --encoding=UTF8 --locale=C -D $DATA_DIR

exit

sudo vi /etc/systemd/system/postgresql.service
i
#--put:
[Unit]
Description=PostgreSQL database server
Documentation=man:postgres(1)
[Service]
Type=notify
User=postgres
ExecStart=/usr/local/pgsql/bin/postgres -D /usr/local/pgsql/data
ExecReload=/bin/kill -HUP $MAINPID
KillMode=mixed
KillSignal=SIGINT
TimeoutSec=0
[Install]
WantedBy=multi-user.target
#--
ESC wq

sudo systemctl daemon-reload
sudo systemctl start postgresql
sudo systemctl enable postgresql
# check
sudo systemctl status postgresql

sudo vi /usr/local/pgsql/data/postgresql.conf
i
#--put:
set logging_collector to on
ESC wq

sudo systemctl restart postgresql

createdb --encoding=UTF8 --locale=C APPLICATION_NAME

psql APPLICATION_NAME
\q




```
