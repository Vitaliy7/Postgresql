# Postgresql
## Домашнее задание по теме _Postgres: Backup + Репликация_
> Что нужно сделать?  

>* настроить hot_standby репликацию с использованием слотов  
>* настроить правильное резервное копирование  
> Для сдачи работы присылаем ссылку на репозиторий, в котором должны обязательно быть  
>* Vagranfile (2 машины)  
>* плейбук Ansible  
>* конфигурационные файлы postgresql.conf, pg_hba.conf и recovery.conf,  
>* конфиг barman, либо скрипт резервного копирования.  
> Команда "vagrant up" должна поднимать машины с настроенной репликацией и резервным копированием.  
> Рекомендуется в README.md файл вложить результаты (текст или скриншоты) проверки работы репликации и резервного копирования.  

### Решение ДЗ.  
Стенд с репликацией и резервным копированием разворачивается автоматически при помощи _Vagrant+Ansible_.  
Задание сделано по методичке с небольшими изменениями.  
После разворачивания всех машин выполняем:  
1. Подключаемся к _node1_, создаем базу _otus_test_ и выводим список баз:

![](https://github.com/Vitaliy7/Postgresql/blob/main/screenshots/node1.png?raw=true)  

Как видим, база создалась. Проверяем _hot_standby_ репликацию, для этого подключаемся к _node2_ и смотрим список БД:

![](https://github.com/Vitaliy7/Postgresql/blob/main/screenshots/node2.png?raw=true)

Репликация отработала корректно.

2. Проверяем работу _barman_. Заходим по _ssh_ на хост _barman_ и смотрим правильность работы утилиты:

![](https://github.com/Vitaliy7/Postgresql/blob/main/screenshots/barman1.png?raw=true)

_Barman_ успешно сконфигурирован и настроен для создания бекапов с БД _node1_.  
Создаем резервную копию командой ```barman backup node1```. Проверим восстановление из бекапа.  
Для этого удалим базы _otus_ и _otus_test_ на _node1_  

![](https://github.com/Vitaliy7/Postgresql/blob/main/screenshots/node1,1.png?raw=true)

На хосте _barman_ проверяем список бекапов ```barman list-backup node1``` и запускаем восстановление:

![](https://github.com/Vitaliy7/Postgresql/blob/main/screenshots/barman2.png?raw=true)

Видим, что восстановление успешно завершено и далее на хосте _node1_ перезапускаем _postgresql_ сервер и снова проверяем список БД:

![](https://github.com/Vitaliy7/Postgresql/blob/main/screenshots/node1,2.png?raw=true)

Восстановление резервной копии при помощи _barman_ выполнено корректно.
