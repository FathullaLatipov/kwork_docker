# step1 - step2 Создание постгрес дата и master-slave
Step 1 mkdir postgres-replication && cd postgres-replication
Step 2 mkdir -p data/master data/slave
# Создание docker-compose.yml
Step 3 create docker-compose.yml
# Создание .env файла
Step 4 create .env 
# Запускает все команды внутри docker-compose
Step 5 docker-compose up -d master
# Создания дубликация БД(часто всего используется для востановления)
Step 6 (sudo su)pg_dump -U <username> -h <hostname> <database> > dump.sql
# То что мы скопировали копируем внутри Docker-container(резервная копия)
Step 7 docker cp dump.sql postgres-replication_master_1:/tmp/
# Зайти в docker через терминал
Step 8 docker-compose exec master bash
# Берем из резервного копии все данные из БД
Step 9 psql -U $POSTGRES_USER -d $POSTGRES_DB -f /tmp/dump.sql
# Назначиваем роль для postgres
Step 10 psql -U <username> -h <hostname> -c "CREATE ROLE replication LOGIN REPLICATION PASSWORD '<password>';" <database>
# Запускаем второй контейнер 
Step 11 docker-compose up -d slave
# Проверяем log файлы
Step 12 docker-compose logs -f slave
# Проверям БД и информации внутри БД
Step 13 docker exec -it my-postgres-container psql -U postgres postgres_db