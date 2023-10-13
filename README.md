# Учебный проект: разворачивание проекта на сервере - Kittigram
### Описание:
**Kittygram** - веб сервис предназначеный для всех, кто любит котиков и  хочет поделиться фотографиями и достижениями своих питомцев с другими пользователями.

### Используемые технологии:
![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)
![Django](https://img.shields.io/badge/django-%23092E20.svg?style=for-the-badge&logo=django&logoColor=white)
![DjangoREST](https://img.shields.io/badge/DJANGO-REST-ff1709?style=for-the-badge&logo=django&logoColor=white&color=ff1709&labelColor=gray)
![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white)
![Gunicorn](https://img.shields.io/badge/gunicorn-%298729.svg?style=for-the-badge&logo=gunicorn&logoColor=white)
### Запуск проекта на удаленном сервере:
1. Подключиться к удалённому серверу:
```
$ ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом_без_расширения login@ip
```
2. Кланировать репозиторий:
```
$ git clone git@github.com:evgenii-erokhin/infra_sprint1.git
```
3. Создать и активировать виртуальное окружение:
```
$ cd infra_sprint1/backend/
$ python -m venv venv
$ source venv/bin/activate
```
4. Установить зависимости:
```
pip install -r requirements.txt
```
5. Выполнить миграции:
 ```
python manage.py migrate
```
6. Устанвоить Gunicorn:
 ```
pip install gunicorn==20.1.0
```
7. Создать юнит для Gunicorn:
```
sudo nano /ect/systemd/system/gunicorn_kittygram.service
```
Прописать:
```
[Unit]
Description=<описание юнита>
After=network.target 

[Service]
User=<Имя пользователя> 

WorkingDirectory=<Путь к директории проекта>

ExecStart=<директория-с-проектом>/<путь-до-gunicorn-в-виртуальном-окружении> --bind 0.0.0.0:8000 kyttygram_backend.wsgi

[Install]
WantedBy=multi-user.target
```
8. Запустить созданый юнит и добавить процесс Gunicorn в список автозапуска операционной системы на удалённом сервере:
```
sudo systemctl start gunicorn_kittygram
sudo systemctl enable gunicorn   
```
9. Установка и запуск Nginx:
```
sudo apt install nginx -y
sudo systemctl start nginx
```
10. Настройка и запуск файрвола:
```
sudo ufw allow 'Nginx Full'
sudo ufw allow OpenSSH
sudo ufw enable
```
11. Собрать статику фронтенд-приложения и разместить её в той директории, которую Nginx использует по умолчанию для доступа к статическим файлам:
    * Перейти в директорию `/infra_sprint1/frontend/` и выполнить команду:

      ```
      npm run build
      ```
    * Скопировать созданую папку в `/var/www/`

      ```
      sudo cp -r /home/<your_username>r/infra_sprint1/frontend/build/. /var/www/kittygramm/ 
      ```
12. Описать конфигурациооные настройки Nginx:
```
 sudo nano /etc/nginx/sites-enabled/default
```
```
server {
    server_name ***.***.***.*** kittygramprojects.hopto.org;

    location /api/ {
        proxy_pass http://127.0.0.1:8080;
    }

    location /admin/ {
        proxy_pass http://127.0.0.1:8080;
    }

    location /media/ {
        alias /var/www/kittygram/media/;
    }

    location / {
        root   /var/www/kittygram;
        index  index.html index.htm;
        try_files $uri /index.html;
    }
}
```
13. Проверить файл конфигурации на ошибки:
```
sudo nginx -t 
```
14. Перезагрузить Nginx:
```
sudo systemctl reload nginx
```  
15. Собрать статику бекенда и перенести в директорию с которой работает Nginx:
```
python manage.py collectstatic
```
```
sudo cp -r infra_sprint1/backend/static_backend/ /var/www/kittygram/
```
16. Создать директорию `media` в директории `/var/www/kittygramm/`для хранения пользовательских картинок
17. В директории `/infra_sprint1/backend/kittygram/backend/` создать `.env`  и поместить переменные окружения `SECRET_KEY` И `DEBUG` в формате `<КЛЮЧ: значение>`
    
18. Получить SSL сертификат:
    + Установить пакетный менеджер snapd:
      ```
      sudo apt install snapd
      ```
    + Установить и обновить зависимости для пакетного менеджера snap:
      ```
      sudo snap install core; sudo snap refresh core  
      ```
    + Установить пакет certbot:
      ```
      sudo snap install --classic certbot
      ```
    + Создать ссылки на certbot в системной директории:
      ```
      sudo ln -s /snap/bin/certbot /usr/bin/certbot
      ```
    + Запустить certbot и получить SSL-сертификат:
      ```
      sudo certbot --nginx
      ```
    + Перезагрузить конфигурацию Nginx:
      ```
      sudo systemctl reload nginx
      ```
