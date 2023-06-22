# Учебный проект: разворачивание проекта на сервере - Kittigram
### Описание:
**Kittygram** -- веб сервис предназначеный для всех, кто любит котиков и  хочет поделиться фотографиями и достижениями своих питомцев с другими пользователями.

### Используемые технологии:
![image](https://img.shields.io/badge/Python-FFD43B?style=for-the-badge&logo=python&logoColor=blue)
![image](https://img.shields.io/badge/Django-092E20?style=for-the-badge&logo=django&logoColor=green)
![image](https://img.shields.io/badge/django%20rest-ff1709?style=for-the-badge&logo=django&logoColor=white)
![image](https://img.shields.io/badge/JWT-000000?style=for-the-badge&logo=JSON%20web%20tokens&logoColor=white)
![image]()
### Запуск проекта на удаленном сервере:
1. Подключиться к удалённому серверу:
```
$ ssh -i путь_до_файла_с_SSH_ключом/название_файла_с_SSH_ключом_без_расширения login@ip
```
2. Кланировать репозиторий:
```
$ git clone git@github.com:******
```
3. Создать и активировать виртуальное окружение:
```
$ cd infra_sprint1/backend/
$ python -m venv venv
$ source venv/bin/activate
```
4. Установить зависимости:
```
(venv) $ pip install -r requirements.txt
```
5. Выполнить миграции:
 ```
(venv) $ python manage.py migrate
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
11. 
