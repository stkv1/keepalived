# Домашнее задание к занятию 1 «Disaster recovery и Keepalived»

# Козлов Станислав

## Задание 1

Дана схема для Cisco Packet Tracer, рассматриваемая в лекции.

На данной схеме уже настроено отслеживание интерфейсов маршрутизаторов Gi0/1 (для нулевой группы)
Необходимо аналогично настроить отслеживание состояния интерфейсов Gi0/0 (для первой группы).

Для проверки корректности настройки, разорвите один из кабелей между одним из маршрутизаторов и Switch0 и запустите ping между PC0 и Server0.
На проверку отправьте получившуюся схему в формате pkt и скриншот, где виден процесс настройки маршрутизатора.

Проверяем статус трекинга портов в группе:

![text](https://github.com/stkv1/keepalived/blob/main/img/103.PNG)

Включаем трекинг:

![text](https://github.com/stkv1/keepalived/blob/main/img/104.PNG)

Проверяем, что в статусе появился параметр
Track interface GigabitEthernet0/1 State Up Decrement 10

![text](https://github.com/stkv1/keepalived/blob/main/img/105.PNG)

Аналогично делаем для второго маршрутизатора


## Задание 2

Запустите две виртуальные машины Linux, установите и настройте сервис Keepalived как в лекции, используя пример конфигурационного файла.

Настройте любой веб-сервер (например, nginx или simple python server) на двух виртуальных машинах

Напишите Bash-скрипт, который будет проверять доступность порта данного веб-сервера и существование файла index.html в root-директории данного веб-сервера.

Настройте Keepalived так, чтобы он запускал данный скрипт каждые 3 секунды и переносил виртуальный IP на другой сервер, если bash-скрипт завершался с кодом, отличным от нуля (то есть порт веб-сервера был недоступен или отсутствовал index.html). Используйте для этого секцию vrrp_script

На проверку отправьте получившейся bash-скрипт и конфигурационный файл keepalived, а также скриншот с демонстрацией переезда плавающего ip на другой сервер в случае недоступности порта или файла index.html

По какой-то причине не происходит переключения, хотя скрипты отрабатываются корректно:


![text](https://github.com/stkv1/keepalived/blob/main/img/210.PNG)

Версия ОС - Ubuntu 20.04

Keepalived 2.0.19

Скриншоты, где видно, что на мастере остановлен Nginx, а на Backup-е запущен:

![text](https://github.com/stkv1/keepalived/blob/main/img/206.PNG)

![text](https://github.com/stkv1/keepalived/blob/main/img/207.PNG)

Переключение не происходит:

![text](https://github.com/stkv1/keepalived/blob/main/img/211.PNG)

![text](https://github.com/stkv1/keepalived/blob/main/img/212.PNG)

![text](https://github.com/stkv1/keepalived/blob/main/img/213.PNG)


Сами скрипты отрабатывают корректно, права выставлены 755, пути до скриптов верные, пользователь, указанный в конфиге, соответствует владельцу скриптов

Решение найти не удалось

## Задание 3*
Изучите дополнительно возможность Keepalived, которая называется vrrp_track_file

Напишите bash-скрипт, который будет менять приоритет внутри файла в зависимости от нагрузки на виртуальную машину (можно разместить данный скрипт в cron и запускать каждую минуту). Рассчитывать приоритет можно, например, на основании Load average.

Настройте Keepalived на отслеживание данного файла.
Нагрузите одну из виртуальных машин, которая находится в состоянии MASTER и имеет активный виртуальный IP и проверьте, чтобы через некоторое время она перешла в состояние SLAVE из-за высокой нагрузки и виртуальный IP переехал на другой, менее нагруженный сервер.

Попробуйте выполнить настройку keepalived на третьем сервере и скорректировать при необходимости формулу так, чтобы плавающий ip адрес всегда был прикреплен к серверу, имеющему наименьшую нагрузку.
На проверку отправьте получившийся bash-скрипт и конфигурационный файл keepalived, а также скриншоты логов keepalived с серверов при разных нагрузках

