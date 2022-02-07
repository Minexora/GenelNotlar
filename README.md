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
