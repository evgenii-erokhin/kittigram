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
3. Кланировать репозиторий:
```
$ git clone git@github.com:******
```
5. Создать и активировать виртуальное окружение:
```
$ cd 06_Kittygram/backend/
$ python -m venv venv
$ source venv/bin/activate
```
5. Установить зависимости:
```
(venv) $ pip install -r requirements.txt
```
7. Выполнить миграции:
 ```
(venv) $ python manage.py migrate
```
9. Устанвоить Gunicorn:
 ```
pip install gunicorn==20.1.0
```
11. Создать unit для Gunicorn:
```
sudo nano /ect/systemd/system/gunicorn_kittygram.service
```
