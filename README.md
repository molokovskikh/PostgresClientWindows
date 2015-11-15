# Клиент СУБД Postgres для ОС Windows
 
 Нативные компоненты для доступа к серверу <i><b>Postgres SQL</b></i>, в репозитории включены основные утилиты:<br>
 <b>psql</b> - косноль для выполнения запросов <i>(help keyword = <b>--help</b>)</i><br>
 <b>pg_dump</b> - утилита для создания бэкапа БД <i>(help keyword = <b>--help</b>)</i><br>
 <b>pg_restore</b> - утилита для восстановление БД из бэкапа <i>(help keyword = <b>--help</b>)</i><br>

* Основные команды утилиты <b>psql</b>

* <b>psql -l</b> - список баз данных.
* <b>psql -d dbname</b> - подключение к БД dbname.
* <b>psql -f file.sql</b> - выполнение команд из файла file.sql.
* <b>psql -U postgres -d dbname -c "CREATE TABLE test(some_id serial PRIMARY KEY, some_text text);"</b> - выполнение команды в базе dbname.
* <b>psql -d dbname -H -c "SELECT * FROM test" -o test.html</b> - вывод результата запроса в html-файл.

* Команды консоли <b>psql</b>:<br>
<b>\c dbname</b> - подсоединение к БД dbname.<br>
<b>\l</b> - список баз данных.<br>
<b>\dt</b> - список всех таблиц.<br>
<b>\d table</b> - структура таблицы table.<br>
<b>\du</b> - список всех пользователей и их привилегий.<br>
<b>\dt+</b> - список всех таблиц с описанием.<br>
<b>\dt *s*</b> - список всех таблиц, содержащих s в имени.<br>
<b>\i FILE</b> - выполнить команды из файла FILE.<br>
<b>\o FILE</b> - сохранить результат запроса в файл FILE.<br>
<b>\a</b> - переключение между режимами вывода: <b>с/</b> без выравнивания.<br>

* Примеры запросов для консоли <b>psql</b>:<br><br>
Просмотр списка и путей к конфигурационным файлам:<br>
<b>psql > <code>SELECT name, setting FROM pg_settings WHERE category = 'File Locations';</code></b><br>
или список всех конфигурационных параметров<br>
<b>psql > show all;</b><br><br>
Список активных соединений с информацией о: pid процесса, выполняющегося запроса, пользователя, базы данных.<br>
<b>psql > <code>SELECT * FROM pg_stat_activity;</code></b><br><br>
Создание индексов:<br>
<b>primary key</b><br>
<b>psql > <code>ALTER TABLE tableName ADD PRIMARY KEY (id);</code></b><br>
<b>unique index</b><br>
<b>psql > <code>CREATE UNIQUE INDEX indexName ON tableName (columnNames);</code></b><br>


* Создание бекапа утилитой <b>pg-dump</b>:<br><br>
  Создание бекапа базы <i>mydb</i>, в сжатом виде:<br>
<b>pg_dump -h localhost -p 5432 -U someuser -F c -b -v -f mydb.backup mydb</b><br><br>
  Создание бекапа базы <i>mydb</i>, в виде обычного текстового файла, включая команду для создания БД:<br>
<b>pg_dump -h localhost -p 5432 -U someuser -C -F p -b -v -f mydb.backup mydb</b><br><br>
  Создание бекапа базы mydb, в сжатом виде, с таблицами которые содержат в имени payments:<br>
<b>pg_dump -h localhost -p 5432 -U someuser -F c -b -v -t *payments* -f payment_tables.backup mydb</b><br><br>
  Дамп данных только одной, конкретной таблицы. Если нужно создать резервную копию нескольких таблиц, то имена этих таблиц перечисляются с помощью ключа -t для каждой таблицы.<br>
<b>pg_dump -a -t table_name -f file_name database_name</b><br><br>

  Список наиболее часто используемых опций <b>pg_dump</b>:<br><br>
<b>-h host</b> - хост, если не указан то используется localhost или значение из переменной окружения PGHOST.<br>
<b>-p port</b> - порт, если не указан то используется 5432 или значение из переменной окружения PGPORT.<br>
<b>-u</b> - пользователь, если не указан то используется текущий пользователь, также значение можно указать в переменной окружения PGUSER.<br>
<b>-a, --data-only</b> - дамп только данных, по-умолчанию сохраняются данные и схема.<br>
<b>-b</b> - включать в дамп большие объекты (blog'и).<br>
<b>-s, --schema-only</b> - дамп только схемы.<br>
<b>-C, --create</b> - добавляет команду для создания БД.<br>
<b>-c</b> - добавляет команды для удаления (drop) объектов (таблиц, видов и т.д.).<br>
<b>-O</b> - не добавлять команды для установки владельца объекта (таблиц, видов и т.д.).<br>
<b>-F, --format {c|t|p}</b> - выходной формат дампа, custom, tar, или plain text.<br>
<b>-t, --table=TABLE</b> - указываем определенную таблицу для дампа.<br>
<b>-v, --verbose</b> - вывод подробной информации.<br>
<b>-D, --attribute-inserts</b> - дамп используя команду INSERT с списком имен свойств.<br>


* В <b><i>PostgreSQL</i></b> есть две утилиты для восстановления базы из бекапа.<br>
<b>psql</b> - восстановление бекапов, которые хранятся в обычном текстовом файле (plain text);<br>
<b>pg_restore</b> - восстановление сжатых бекапов (<i>tar</i>);<br><br>
  Восстановление всего бекапа с игнорированием ошибок:<br>
<b>psql -h localhost -U someuser -d dbname -f mydb.sql</b><br><br>
  Восстановление всего бекапа с остановкой на первой ошибке:<br>
<b>psql -h localhost -U someuser --set ON_ERROR_STOP=on -f mydb.sql</b><br><br>
  Для восстановления из tar-арихива нам понадобиться сначала создать базу с помощью <code>CREATE DATABASE mydb;</code> (если при создании бекапа не была указана опция <b>-C</b>) и восстановить:<br>
<b>pg_restore --dbname=mydb --jobs=4 --verbose mydb.backup</b><br>

  Начиная с версии <b>9.2</b> можно восстановить только структуру таблиц с помощью опции <i>--section</i>:<br>
<code># создаем БД</code><br>
<code>CREATE DATABASE mydb2;</code><br>
восстанавливаем<br>
<b>pg_restore --dbname=mydb2 --section=pre-data --jobs=4 mydb.backup</b><br>


<i>Источник нативных компонент:</i><br>
http://get.enterprisedb.com/postgresql/postgresql-9.4.5-1-windows-x64-binaries.zip

<i>Больше информации можно получить по ссылке</i><br>http://postgresql.ru.net/manual/index.html
