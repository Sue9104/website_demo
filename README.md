# Build Website with Django and Vue

```sh
django-admin startproject demo
```


## Frontend using Vue

```
vue init webpack frontend
cd frontend && npm run dev
npm run build
```


## Backend Using Django

```
cd demo
python manage.py startapp backend
```


## Integrate Vue and Django

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

### axios

1. install axios

```
npm install --save axios vue-axios
```

2. edit vue

```
export default {
  name: 'app',
  data () {
    return {
      string_from_api: null
    }
  },
  mounted () {
    this.axios
    .get('api/666')
    .then(response => (this.string_from_api = response.data))
    .catch( function(error) {
      console.log(error);
    })
  }
}
```

### PostgreSQL

1. install psycopy

```
pip install psycopy
```

2. postgresql command

  - add role
  ```
  create role demo with 
  login
  createdb
  create role;
  alter use demo with password '123456';
  create database myproject;
  grant all privileges on database myproject to demo;
  ```

  - enable login (peer to trust in /etc/postgresql/11/main/pg_hba.conf)
  ```
  # change peer to trust in /etc/postgresql/11/main/pg_hba.conf
  ```

3. edit DATABASES in setting.py
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'myproject',
        'USER': 'demo',
        'PASSWORD': '123456',
        'HOST': '127.0.0.1',
        'client_encoding': 'UTF8',
        'timezone': 'Asia/Shanghai',
    }
}
```

4. edit model.py
```
class Book(models.Model):
    book_name = models.CharField(max_length=64)
    add_time = models.DateTimeField(auto_now_add=True)

    def __unicode__(self):
        return self.book_name
```

5. migrate

```
python manage.py makemigrations myapp
python manage.py migrate
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
