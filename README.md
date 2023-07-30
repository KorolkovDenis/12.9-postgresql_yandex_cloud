### Домашнее задание к занятию «Базы данных в облаке» - Корольков Денис


### Задание 1

#### Создание кластера
1. Перейдите на главную страницу сервиса Managed Service for PostgreSQL.
1. Создайте кластер PostgreSQL со следующими параметрами:
- класс хоста: s2.micro, диск network-ssd любого размера;
- хосты: нужно создать два хоста в двух разных зонах доступности и указать необходимость публичного доступа, то есть публичного IP адреса, для них;
- установите учётную запись для пользователя и базы.

Остальные параметры оставьте по умолчанию либо измените по своему усмотрению.

* Нажмите кнопку «Создать кластер» и дождитесь окончания процесса создания, статус кластера = RUNNING. Кластер создаётся от 5 до 10 минут.

#### Подключение к мастеру и реплике 

* Используйте инструкцию по подключению к кластеру, доступную на вкладке «Обзор»: cкачайте SSL-сертификат и подключитесь к кластеру с помощью утилиты psql, указав hostname всех узлов и атрибут ```target_session_attrs=read-write```.

* Проверьте, что подключение прошло к master-узлу.
```
select case when pg_is_in_recovery() then 'REPLICA' else 'MASTER' end;
```
* Посмотрите количество подключенных реплик:
```
select count(*) from pg_stat_replication;
```

### Проверьте работоспособность репликации в кластере

* Создайте таблицу и вставьте одну-две строки.
```
CREATE TABLE test_table(text varchar);
```
```
insert into test_table values('Строка 1');
```

* Выйдите из psql командой ```\q```.

* Теперь подключитесь к узлу-реплике. Для этого из команды подключения удалите атрибут ```target_session_attrs```  и в параметре атрибут ```host``` передайте только имя хоста-реплики. Роли хостов можно посмотреть на соответствующей вкладке UI консоли.

* Проверьте, что подключение прошло к узлу-реплике.
```
select case when pg_is_in_recovery() then 'REPLICA' else 'MASTER' end;
```
* Проверьте состояние репликации
```
select status from pg_stat_wal_receiver;
```

* Для проверки, что механизм репликации данных работает между зонами доступности облака, выполните запрос к таблице, созданной на предыдущем шаге:
```
select * from test_table;
```

*В качестве результата вашей работы пришлите скриншоты:*

*1) Созданной базы данных;*
*2) Результата вывода команды на реплике ```select * from test_table;```.*

Ответ:

Зашел на MASTER, создал таблицу и несколько строк в не:

```
psql "host=rc1a-efhebgdyfp3vnq6s.mdb.yandexcloud.net,rc1b-tkmdbfmehw72mvs7.mdb.yandexcloud.net \
    port=6432 \
    sslmode=verify-full \
    dbname=db1_korolkov \
    user=korokov \
    target_session_attrs=read-write"
```
![screen1](https://github.com/KorolkovDenis/12.9-postgresql_yandex_cloud/blob/main/screenshots/screen1.jpg)
```
SELECT version();
```
![screen2](https://github.com/KorolkovDenis/12.9-postgresql_yandex_cloud/blob/main/screenshots/screen2.jpg)

Проверьте, что подключение прошло к master-узлу.
```
select case when pg_is_in_recovery() then 'REPLICA' else 'MASTER' end;
```
Посмотрите количество подключенных реплик:
```
select count(*) from pg_stat_replication;
```
![screen3](https://github.com/KorolkovDenis/12.9-postgresql_yandex_cloud/blob/main/screenshots/screen3.jpg)

Создам таблицу и добавлю несколько строк в нее:

![screen4](https://github.com/KorolkovDenis/12.9-postgresql_yandex_cloud/blob/main/screenshots/screen4.jpg)

Теперь поработаю с Репликой. Для этого из команды подключения удалю атрибут target_session_attrs и в параметре атрибут host передаю только имя хоста-реплики. 

![screen5](https://github.com/KorolkovDenis/12.9-postgresql_yandex_cloud/blob/main/screenshots/screen5.jpg)

Это инфа по созданным у нас БД и таблицам:

![screen6](https://github.com/KorolkovDenis/12.9-postgresql_yandex_cloud/blob/main/screenshots/screen6.jpg)

Если что то пошло не так, шде то опечатка, как у меня в логине ))), то можно скопировать весь скрипт для подключения с самого кластера тут:

![screen7](https://github.com/KorolkovDenis/12.9-postgresql_yandex_cloud/blob/main/screenshots/screen7.jpg)

Жмем подключиться и копируем:

![screen8](https://github.com/KorolkovDenis/12.9-postgresql_yandex_cloud/blob/main/screenshots/screen8.jpg)


### Задание 2*

Создайте кластер, как в задании 1 с помощью Terraform.


*В качестве результата вашей работы пришлите скришоты:*

*1) Скриншот созданной базы данных.*
*2) Код Terraform, создающий базу данных.*

---

Задания, помеченные звёздочкой, — дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.



[Cсылка на google docs по «Резервное копирование баз данных»](https://docs.google.com/document/d/1tT7QFVqyYvcGOqLPuzfkn1oabXEZR2cr/edit?usp=sharing&ouid=104113173630640462528&rtpof=true&sd=true)

