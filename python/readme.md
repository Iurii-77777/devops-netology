## **Задание 1**

![Screenshot](1__.jpg)

## **Задание 2**

### **Мой скрипт:**
```
#!/usr/bin/env python3
import os

bash_command = ["cd ~/devops-netology", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
link = ["cd ~/devops-netology", "pwd"]
link_absolut = os.popen(' && '.join(link)).read()
prepare_link = link_absolut.replace('\n', '')
for result in result_os.split('\n'):
    if result.find('изменено') != -1:
        prepare_result = result.replace('\tизменено:      ', '')
        print(prepare_link + '/' + prepare_result)
```
***
### **Вывод результата:**
```
/home/iurii/devops-netology/devops-netology (изменено содержимое)
/home/iurii/devops-netology/python/devops_3
/home/iurii/devops-netology/python/test
/home/iurii/devops-netology/python/test_777
```
## **Задание 3** 

### **Мой скрипт:**
```
#!/usr/bin/env python3
import os

vvod = input('Введите имя каталога: ')
bash_command = ["cd ~/"+vvod, "git status 2>&1"]
result_os = os.popen(' && '.join(bash_command)).read()
link = ["cd ~/"+vvod, "pwd"]
link_absolut = os.popen(' && '.join(link)).read()
prepare_link = link_absolut.replace('\n', '')
for result in result_os.split('\n'):
    if result.find('изменено') != -1:
        prepare_result = result.replace('\tизменено:      ', '')
        print('\n' + prepare_link + '/' + prepare_result)
    elif result.find('fatal') != -1:
        print('\nДанный каталог не является Git репозиторием')
```
***
### **Вывод результата (при вводе "не Git" каталога):**
```
Введите имя каталога: >? Видео

Данный каталог не является Git репозиторием

```
## **Задание 4**

### **Мой скрипт:**
```
import socket
import time as t

site = {'drive.google.com':'0.0.0.0', 'mail.google.com':'0.0.0.0', 'google.com':'0.0.0.0'}

a = 1
timeout = 3
i = 0
while 1 == 1:
    for host in site:
        ip = socket.gethostbyname(host)
        if ip != site[host]:
            if a == 1 and i != 1:
                print(' [ERROR] ' + str(host) + ' IP mistmatch: ' + site[host] + ' ' + ip)
            site[host] = ip
            i += 1
        if i >= 50:
            break
        t.sleep(timeout)
```
***
### **Вывод результата:**
```
 [ERROR] drive.google.com IP mistmatch: 0.0.0.0 173.194.221.194
 [ERROR] google.com IP mistmatch: 0.0.0.0 74.125.205.100
 [ERROR] mail.google.com IP mistmatch: 0.0.0.0 173.194.222.18
```


