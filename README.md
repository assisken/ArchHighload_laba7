# Архитектура высоконагруженных система. ДЗ №7
## Два сервиса на разных языках программирования


## Цель:
> Взять сервер из 1-го ДЗ и:
> 1. Создать ручку PUT/POST для сохранения погоды на сервер.
> 2. Настроить сохранение всех данных о погоде в redis.
> 3. Исправить ручку GET так, чтобы проверка данных проходила через redis. Если данных в redis нет, то их необходимо запросить у сервиса-источника погоды, сохранить в redis и отдать пользователю.
> 4. Нужно настроить репликацию и шардирование для redis. Должно быть как минимум два шарда redis.
> * можно использовать *Redis Cluster* или *Redis/Sentinel/temproxy* или *mcrouter/memcahed*
> * проверить отказоустойчивость при падении мастера, наличие равномерного шардирования

--------------


**Шардирование** - метод разделения и хранения единого логического набора данных в виде множества баз данных. Другое определение шардинга — горизонтальное разделение данных.



--------------

## Инструкция по установке:
1. Скачать/стянуть репозиторий
1. Перейти в папку репозитория
1. На сервере выполнить команды для запуска контейнеров:
	```bash
	docker run -dt -v redis.conf:/usr/local/etc/redis/redis.conf -p 5026:6379 -p 15026:16379 --name redis_server6 redis redis-server /usr/local/etc/redis/redis.conf
	docker run -dt -v redis.conf:/usr/local/etc/redis/redis.conf -p 5025:6379 -p 15025:16379 --name redis_server5 redis redis-server /usr/local/etc/redis/redis.conf
	docker run -dt -v redis.conf:/usr/local/etc/redis/redis.conf -p 5024:6379 -p 15024:16379 --name redis_server4 redis redis-server /usr/local/etc/redis/redis.conf
	docker run -dt -v redis.conf:/usr/local/etc/redis/redis.conf -p 5023:6379 -p 15023:16379 --name redis_server3 redis redis-server /usr/local/etc/redis/redis.conf
	docker run -dt -v redis.conf:/usr/local/etc/redis/redis.conf -p 5022:6379 -p 15022:16379 --name redis_server2 redis redis-server /usr/local/etc/redis/redis.conf
	docker run -dt -v redis.conf:/usr/local/etc/redis/redis.conf -p 5021:6379 -p 15021:16379 --name redis_server1 redis redis-server /usr/local/etc/redis/redis.conf
	```
1. 


## Инструментарий:
- GIT (устанавливается командой `sudo apt install git -y`)
- Установленные модули:
	+ FastAPI `sudo pip3 install fastapi`
	+ Unicorn `sudo pip3 install uvicorn`
	+ redis-py-cluster `sudo pip3 install redis-py-cluster` [ссылка](https://pypi.org/project/redis-py-cluster/)
	+ ~~ранее был использован redis, но он [не поддерживает работу с кластером](https://github.com/andymccurdy/redis-py#cluster-mode)~~
