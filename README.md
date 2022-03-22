# GenelNotlar
Tüm Yazılım Dillerinden Notlar

## Python File Okuma Yazma
- Pythonda dosya okuma için;
  ```py
  import os
  
  with open('file path ve adı', 'dosya açama modu') as file:
    file.read()
  ```
- Pythonda dosya yazma
  ```py
  import os

  with open('file path ve adı', 'dosya açama modu') as file:
    file.write('Yazılacak data')
  ```
## Django Multiple Database Kullanımı
- settings.py dosyasında databasleraşağıdaki gibi tanımlanır;
  ```py
    DATABASES = {
      'default': {
          'ENGINE': 'sql_server.pyodbc',
          'NAME': 'Name',
          'USER': 'User',
          'PASSWORD': 'Password',
          'HOST': 'Host',
          'PORT': '1433',
          'OPTIONS': {
              'driver': 'ODBC Driver 17 for SQL Server'
          },
      },
      'second': {
          'ENGINE': 'sql_server.pyodbc',
          'NAME': 'Name',
          'USER': 'User',
          'PASSWORD': 'Password',
          'HOST': 'Host',
          'PORT': '1433',
          'OPTIONS': {
              'driver': 'ODBC Driver 17 for SQL Server'
          },
      }

    }
  ```
- views.py dosyasında import edilip kullanımı aşağıdaki gibidir.
  ```py
    from django.db import connection, connections
    
    procedure(connections['second'], "exec procedure ", top=1)

    def procedure(connection, procedure_string, top=None):
    cursor = connection.cursor()
    cursor.execute(procedure_string)

    if cursor.description is None:
        return None

    columns = [column[0] for column in cursor.description]
    result = list()
    count = 0
    for i in cursor.fetchall():
        count += 1
        result.append(dict(zip(columns, i)))
        if top and top >= count:
            break

    cursor.close()
    return result
  ```
## Gunicorn Environmentli kullanımı
 - Gunicorn ile eventlet ve env kullanımı aşağıdaki gibidir.
  ```
   gunicorn --env DJANGO_SETTINGS_MODULE=mhatsappApi.settings --worker-class eventlet -w 1 mhatsappApi.wsgi:application

   gunicorn --env DJANGO_SETTINGS_MODULE=mhatsappApi.settings --worker-class eventlet -w 1 mhatsappApi.wsgi:application --bind 0.0.0.0:8000
  ```
## Ssh Kullanımı
 - Key üretmek için;
 ```
  ssh-keygen
 ```
 - Oluşturulan keyi sunucuya kopyalamak için;
 ```
  ssh-copy-id kullanıcı@ip
 ```
 - VS Code içerisinden remote ssh tool ile bağlantı için;
 ```
  ssh kullanıcı@ip -A 
 ```
## Nginx kullanımı
 - /etc/nginx/sites-available adresine gidilir.
 - fqdn.domain.conf adında bir dosya oluşturulur.
 - Nginx yapılandırma örneği aşağıdaki gibidir.Oluşturulan dosyanın içerisine yazılır.
  ```
   upstream socketio_nodes {
      ip_hash;

      server 127.0.0.1:8000;
   }

   server {
      listen 80;
      server_name fqdn.domain.com;

      location / {
          include proxy_params;
          proxy_pass http://127.0.0.1:8000;
      }

      location /socket.io {
          include proxy_params;
          proxy_http_version 1.1;
          proxy_buffering off;
          proxy_set_header Upgrade $http_upgrade;
          proxy_set_header Connection "Upgrade";
          proxy_pass http://127.0.0.1:8000/socket.io;
      }
   }
  ```
 - Nginx başlatmak için;
 ```
  #Start
  sudo systemctl start nginx
  
  #Restart
  sudo systemctl restart nginx
  
  #Startup
  sudo ln -s /etc/nginx/sites-available/your_domain /etc/nginx/sites-enabled/
  
  #Test
  sudo nginx -t
 ```
