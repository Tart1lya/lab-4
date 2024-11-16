## Отчёт по лабораторной работе №4

Нужно решить следующую задачу:

Запустить в контейнере приложение “aafire”. Обратите внимание, что оно бесконечное и контейнер не будет автоматически отключаться.  
Приложить скриншот в процессе работы контейнера.  

Далее в рамках лабораторной работы необходимо самостоятельно настроить сеть между двумя контейнерами, также как в предыдущей работе вы настраивали связь между двумя виртуальными машинами.  

Для проверки сети между контейнерами вам потребуется утилита ping. Поскольку контейнеры очень маленькие и в них нет ничего лишнего (по сравнению с виртуальными машинами) - ping там не установлен. В вашем образе нужно будет установить пакет с этой утилитой, помимо aafire.  

Далее запустите два контейнера с aafire и оставьте их в работающем состоянии.  
Откройте ещё одно окно терминала и создайте сеть при помощи команды 
```
docker network create myNetwork
```
После этого нужно будет подключить контейнеры к вашей сети. Названия контейнеров можно увидеть при выводе списка действующих контейнеров у вас на машине.
```
docker network connect myNetwork mycontainer1
docker network connect myNetwork mycontainer2
```
Теперь при помощи следующей команды вы можете увидеть настройки созданной вами сети.
```
docker network inspect myNetwork
```
Далее вам нужно самостоятельно протестировать соединение между контейнерами утилитой ping и приложить скриншот.

## Решение

1. Устанавливаем Docker и проверяем, что он установлен с помощью команды
   
   ```
   docker --version
   ```
   ![image](https://github.com/user-attachments/assets/b9d5bf56-8f5d-4d98-b04b-98dacefb1a75)

   После этого запускаем Docker
   
3. Создаем рабочую папку для проекта

   ```
   mkdir my_docker_project
   cd my_docker_project
   ```
   
4. Создаем файл Dockerfile внутри папки my_docker_project

   ```
   touch Dockerfile
   ```
   
5. Открываем Dockerfile для редактирования

   ```
   nano Dockerfile
   ```
   
6. Для выполнения задания пишем следующие команды

   ```
    # Используем минимальный образ Debian
    FROM debian:latest

    # Обновляем пакеты и устанавливаем необходимые утилиты
    RUN apt update && apt install -y aafire iputils-ping

    # Устанавливаем команду по умолчанию для запуска
    CMD ["aafire"]
   ```
   
7. Собираем Docker-образ

   ```
   docker build -t aafire_image .
   ```
   
8. Запускаем первый и второй контейнеры

   ```
   docker run -dit --name mycontainer1 aafire_image
   docker run -dit --name mycontainer2 aafire_image
   ```

  ![image](https://github.com/user-attachments/assets/51cc95d4-8095-4157-8beb-b90865068d55)

9. Создаем пользовательскую сеть Docker

   ```
   docker network create myNetwork
   ```

10. Подключаем оба контейнера к сети

     ```
     docker network connect myNetwork mycontainer1
     docker network connect myNetwork mycontainer2
     ```

11. Открываем терминал внутри первого контейнера

     ```
     docker exec -it mycontainer1 bash
     ```

12. Проверяем связь с контейнером 2: узнаем IP-адрес второго контейнера из вывода команды docker network inspect myNetwork. После этого выполняем:

     ```
     ping <IP-адрес второго контейнера>
     ```

  ![image](https://github.com/user-attachments/assets/a6c615c3-33cf-4eed-8049-c64f88beeedb)

    



