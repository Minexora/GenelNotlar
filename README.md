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
