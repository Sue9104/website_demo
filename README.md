# Build Website with Django and Vue

```sh
django-admin startproject demo
cd demo
python manage.py startapp backend
vue init webpack frontend
cd frontend && npm run dev
```

## Edit setting in Django

```diff
# Edit demo/demo/setting.py

-ALLOWED_HOSTS = []
+ALLOWED_HOSTS = ['*']


 # Application definition
@@ -37,11 +37,13 @@ INSTALLED_APPS = [
     'django.contrib.sessions',
     'django.contrib.messages',
     'django.contrib.staticfiles',
+    'corsheaders',
 ]

 MIDDLEWARE = [
     'django.middleware.security.SecurityMiddleware',
     'django.contrib.sessions.middleware.SessionMiddleware',
+    'corsheaders.middleware.CorsMiddleware',
     'django.middleware.common.CommonMiddleware',
     'django.middleware.csrf.CsrfViewMiddleware',
     'django.contrib.auth.middleware.AuthenticationMiddleware',
     'django.middleware.clickjacking.XFrameOptionsMiddleware',
 ]

+# cors header
+CORS_ORIGIN_ALLOW_ALL = True

TEMPLATES = [
     {
         'BACKEND': 'django.template.backends.django.DjangoTemplates',
-        'DIRS': [],
+        'DIRS': ['frontend/dist'],
         'APP_DIRS': True,
     },
 ]

 STATIC_URL = '/static/'
+STATIC_ROOT = os.path.join(BASE_DIR, "static")
+STATICFILES_DIRS = [
+    os.path.join(BASE_DIR, "frontend/dist/static"),
+]

```

## Start Uwsgi

```
source activate test3
uwsgi --ini uwsgi.ini
```

## Start nginx

```
# collect static
python manage.py collectstatic

# nginx 
sudo cp nginx.conf /etc/nginx/sites-enabled/demo.conf
sudo service nginx restart
```
