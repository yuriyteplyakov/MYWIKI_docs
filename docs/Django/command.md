
- Просмотр комманд
`django-admin`

- Создание проекта
`django-admin startproject jewerly_store`

- Команды утилиты конкретного проекта
`python manage.py`

pip install django-tailwind[reload]

добавляем tailwind в приложения

python manage.py tailwind init

добавляем theme в приложения

в настройках после приложений
TAILWIND_APP_NAME = 'theme'
INTERNAL_IPS = [
    "127.0.0.1",
]

NPM_BIN_PATH = r'C:\Program Files\nodejs\npm.cmd'

python manage.py tailwind install

добавляем 'django_browser_reload' в приложения


# В одном терминале
python manage.py tailwind start

# В другом
python manage.py runserver

python manage.py startapp store


{% url 'cart:cart_detail' %}
позже добавить в base.html



# Активируйте venv
venv\Scripts\activate

# Установите Python-пакет
pip install django-tailwind

# Перейдите в папку theme (это важно!)
cd theme/static_src

# Установите npm зависимости ЛОКАЛЬНО
npm install

# Или используйте команду django-tailwind
python manage.py tailwind install