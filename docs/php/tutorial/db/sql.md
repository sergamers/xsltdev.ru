# Язык запросов SQL

**Система управления базами данных (СУБД)** — это отдельная программа, которая работает как сервер, независимо от PHP.

Создавать свои базы данных, таблицы и наполнять их данными можно прямо из этой же программы, но для выполнения этих операций прежде придётся познакомиться с ещё одним языком программирования — SQL.

**SQL** или Structured Query Language (язык структурированных запросов) — язык программирования, предназначенный для управления данными в СУБД. Все современные СУБД поддерживают SQL.

На языке SQL выражаются все действия, которые можно провести с данными: от записи и чтения данных, до администрирования самого сервера СУБД.

Для повседневной работы совсем не обязательно знать весь этот язык; достаточно ознакомиться лишь с основными понятиями синтаксиса и ключевыми словами. Кроме того, SQL очень простой язык по своей структуре, поэтому его освоение не составит большого труда.

Язык SQL — это в первую очередь язык запросов, а кроме того он очень похож на естественный язык.
Каждый раз, когда требуется прочитать или записать любую информацию в БД, требуется составить корректный запрос. Такой запрос должен быть выражен в терминах SQL.

Например, чтобы вывести на экран все записи из таблицы города, составим такой запрос:

```
ПРОЧИТАТЬ всё ИЗ ТАБЛИЦЫ 'города'
```

Если перевести этот запрос на язык SQL, то корректным результатом будет:

```sql
SELECT * FROM 'cities'
```

Теперь напишем запрос на добавление в таблицу города нового города:

```
ВСТАВЬ В ТАБЛИЦУ 'города' ЗНАЧЕНИЯ 'имя города' = 'Санкт-Петербург'
```

Перевод на SQL:

```sql
INSERT INTO 'cities' SET 'name' = 'Санкт-Петербург'
```

Эта команда создаст в таблице ‘города’ новую запись, где полю ‘имя города’ будет присвоено значение ‘Санкт-Петербург’.

С помощью SQL можно не только добавлять и читать данные, но и:

- удалять и обновлять записи в таблицах;
- создавать и редактировать сами таблицы;
- производить операции над данными: считать сумму, получать самое большое или малое значение, и так далее;
- настраивать работу сервера СУБД.

## MySQL

Существует множество различных реляционных СУБД. Самая известная СУБД — это Microsoft Access, входящая в состав офисного пакета приложений Microsoft Office.

Нет никаких препятствий для использования в качестве СУБД MS Access, но для задач веб-программирования гораздо лучше подходит альтернативная программа — MySQL.

В отличие от MS Access, MySQL абсолютно бесплатна, может работать на серверах с Linux, обладает гораздо большей производительностью и безопасностью, что делает её идеальным кандидатом на роль базы данных в веб-разработке.

Подавляющее большинство сайтов и приложений на PHP используют в качестве СУБД именно MySQL.

## Установка

Если для своей работы вы используете программную среду OpenServer, то этот раздел можно смело пропустить, так как в состав OpenServer уже входит свежая версия MySQL.

Последняя версия MySQL доступна для загрузке по ссылке: [https://dev.mysql.com/downloads/mysql/](https://dev.mysql.com/downloads/mysql/)

На этой странице следует выбрать «MySQL Installer for Windows» и нажать на кнопку «Download» для загрузки.

В процессе установки запомните директорию, куда вы устанавливаете MySQL (скрывается под ссылкой «Advanced options»).

На шаге «Accounts and Roles» установщик потребует придумать пароль для доступа к БД (MySQL Root Password) — обязательно запомните или запишите этот пароль — он вам ещё понадобится.

## Выполнение запросов

По умолчанию, если вы не устанавливали дополнительные программы, у MySQL нет графического интерфейса пользователя. Это значит, что единственный способ работы с ней — это использование командной строки.

1. Откройте командную строку (Выполнить: `cmd.exe`).
2. Перейдите в каталог с установленной MySQL: `cd /d <каталог установки>/bin`.
3. Выполните: `mysql -uroot -p`.
4. Введите пароль, заданный при установке.

Если вы всё выполнили верно, то в командной строке запустится клиент для работы с MySQL (вы поймете это по строке приглашения `mysql>`). С этого момента можно вводить любые SQL запросы, но каждый запрос обязательно должен заканчиваться точкой с запятой `;`

### Оператор SQL create database: создание новой базы данных

Приступим к практике — начнём создавать базу данных для ведения погодного дневника.

Начать следует с создания новой базы данных для нашего сайта.

Новая БД в MySQL создаётся простой командой: `CREATE DATABASE <имя базы данных>`

Так что, выполнив команду:

```sql
CREATE DATABASE weather_diary;
```

MySQL создаст для нас новую БД, в которой будет происходить вся дальнейшая работа.

Это важно: после создания БД её невозможно будет переименовать, а только удалить и создать заново. По этой причине крайне внимательно подойдите к выбору имени для базы данных.

### Оператор create table: создание таблиц

Создав новую БД, сообщим MySQL, что теперь мы собираемся работать именно с ней.

Выбор активной БД выполняется командой: `USE <имя базы>;`

Пришло время создать первые таблицы!

Для ведения дневника по всем правилам, понадобится создать три таблицы: города (`cities`), пользователи (`users`) и записи о погоде (`weather_log`).

В подразделе [«Запись»](index.md#_7) этой главы описано, как должна выглядеть структура таблицы `weather_log`. Переведём это описание на язык SQL:

```sql
CREATE TABLE weather_log (
  id INT AUTO_INCREMENT PRIMARY KEY,
  city_id INT,
  day DATE,
  temperature INT,
  cloud TINYINT DEFAULT 0
);
```

Чтобы ввести многострочную команду в командной строке используйте символ `\` в конце каждой строки (кроме последней).

Теперь создадим таблицу городов:

```sql
CREATE TABLE cities (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name CHAR(128)
)
```

MySQL может показать созданную таблицу, если попросить об этом командой:

```sql
SHOW COLUMNS FROM weather_log;
```

В ответе будут перечислены все поля таблицы, их тип и другие характеристики.

#### Первичный ключ

В примере с созданием новой таблицы при перечислении необходимых полей первым полем идёт `id INT AUTO_INCREMENT PRIMARY KEY`.

Это поле называется первичным ключом. Обязательно создавать первичный ключ в каждой таблице.

_Первичный ключ_ — это особенное поле, в котором сохраняется уникальный идентификатор записи. Он нужен, чтобы у программиста и базы данных всегда была возможность однозначно обратиться к одной конкретной записи для её чтения, обновления или удаления.

Если назначить поле первичным ключом, то БД будет следить за тем, чтобы значение в этом поле больше не повторялось в таблице.

А если ещё и добавить аттрибут `AUTO_INCREMENT`, то MySQL при добавлении новых записей будет заполнять это поле сама. `AUTO_INCREMENT` будет играть роль счётчика — каждая новая запись в таблице получит значение на единицу больше максимального существующего значения.

### Оператор insert into: добавление записи в таблицу

Начнём с добавления новых данных в таблицу. Для добавления записи используется следующий синтаксис:

```
insert into <название таблицы> set <имя столбца1> = <значение2>, <имя столбца2> = <значение2>...
```

В начале добавим город в таблицу городов:

```sql
insert into cities set name = 'Санкт-Петербург'
```

При добавлении записи не обязательно указывать значения для всех полей. Многие из полей имеют значения по умолчанию, которые сами заполняются при сохранении.

Теперь создадим запись о погоде за сегодняшний день.

При определении таблицы `weather_log` мы решили ссылаться на город, путём записи в поле `city_id` идентификатора города из таблицы `cities`. Так как мы только что добавили новый город, ничего не мешает использовать его идентификатор в записи о погоде.

Идентификатором города будет первичный ключ, который также был определён в качестве первого поля таблицы. Нумерация этого поля начинается с единицы, значит первая добавленная запись имеет идентификатор `1`. Зная это, запрос на добавление записи о погоде в Санкт-Петербурге за третье сентября 2017 года выглядит так:

```sql
INSERT INTO weather_log SET city_id = 1, day = '2017-09-03', temperature = 5, cloud = 1;
```

### Оператор select: чтение информации из БД

Для вывода информации из БД используются запросы типа `SELECT`.

В запросе нужно указать имя таблицы, необходимые поля, а также дополнительные параметры (будут рассмотрены в следующем уроке).

```
SELECT <перечисление полей> FROM <имя таблицы>
```

Например, чтобы получить список всех доступных городов:

```sql
SELECT id, name FROM cities
```

Все погодные записи:

```sql
SELECT id, day, city_id, temperature, cloud FROM weather_log
```

Вместо перечисления всех столбцов можно использовать знак звездочки — `*`.

### Оператор update: обновление информации в БД

При добавлении записи очень легко совершить ошибку: сделать опечатку, не указать значение для одного из полей, и так далее.

Естественно, язык SQL предлагает возможности для редактирования уже созданных записей.

Предположим, что при добавлении погодной записи пользователь ошибся и ввёл неверную дату. Чтобы исправить эту ошибку, нужно использовать оператор обновления — `UPDATE`.

Запрос с этим оператором позволяет обновить значение одного или нескольких полей в существующей записи. Выглядит он так:

```
UPDATE <имя таблицы> SET <имя столбца1> = <значение2>, <имя столбца2> = <значение2>... WHERE <имя столбца> = <значение>
```

Но чтобы правильно составить запрос, необходимо определить условие для поиска записи, которую предлагается обновить. В противном случае, если не указать это условие, то будут обновлены абсолютно все записи в таблице.

В качестве такого условия лучше всего использовать первичный идентификатор записи. Поэтому, прежде чем выполнять запрос обновления, нужно выполнить запрос на чтение информации из таблицы, чтобы узнать, под каким идентификатором сохранилась ошибочная запись.

Допустим, этот идентификатор — единица, а правильная дата — пятое сентября 2017 года.

Запрос на обновление:

```sql
UPDATE weather_log SET day = '2017-09-05' WHERE id = 1
```

### Оператор join: объединение записей из двух таблиц

В нашей таблице для хранения погодного дневника город сохраняется как идентификатор, поэтому при обычном чтении данных из этой таблицы вместо названия города стоит непонятное число. Чтобы подставить на место числа действительное значение, а конкретнее — название города, в SQL существуют операторы объединения — `JOIN`.

Поддержка операторов объединения и позволяет базе данных называться реляционной.

Поменяем запрос на показ погодных записей, чтобы он объединял две таблицы, а в поле города показывалось его название, а не идентификатор:

```sql
SELECT day, cities.name, temperature, cloud FROM weather_log JOIN cities ON weather_log.city_id = cities.id
```

Важно усвоить три самых главных момента:

1. При чтении из объединённых таблиц, в перечислении полей после `SELECT` нужно явно указывать в поле имени также имя таблицы, с которой производится объединение.
2. Всегда есть основная таблица (тб1), из которой читается большинство полей и присоединяемая (тб2), имя которой определяется после оператора `JOIN`.
3. Помимо указания имени второй таблицы, обязательно следует указать условие, по которому будет происходить объединение. В этом примере таким условием будет соответствие идентификатора города из тб1 (`weather_log.city_id`) первичному ключу города из тб2 (`cities.id`).
