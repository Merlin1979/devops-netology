# Домашняя работа к занятию "4.2. Использование Python для решения типовых DevOps задач"

## Обязательная задача 1

Есть скрипт:
```python
#!/usr/bin/env python3
a = 1
b = '2'
c = a + b
```

### Вопросы:
| Вопрос  | Ответ |
| ------------- | ------------- |
| Какое значение будет присвоено переменной `c`?  | TypeError: unsupported operand type(s) for +: 'int' and 'str'  |
| Как получить для переменной `c` значение 12?  | c = str(a) + b  |
| Как получить для переменной `c` значение 3?  | c = a + int(b)  |

## Обязательная задача 2
Мы устроились на работу в компанию, где раньше уже был DevOps Engineer. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории, относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся. Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
is_change = False
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
        break
```

### Ответ:
```python
#!/usr/bin/env python3

import os

bash_command = ["cd ~/netology/sysadm-homeworks", "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(prepare_result)
```

### Вывод скрипта при запуске при тестировании:
```
HW-3.9/README.md
HW-4.1/README.md
```

## Обязательная задача 3
1. Доработать скрипт выше так, чтобы он мог проверять не только локальный репозиторий в текущей директории, а также умел воспринимать путь к репозиторию, который мы передаём как входной параметр. Мы точно знаем, что начальство коварное и будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

### Решение:
```python
#!/usr/bin/env python3

import os
import sys

if len(sys.argv) > 1:
    cmd = sys.argv[1]
else:
    cmd = os.getcwd()
bash_command = ["cd "+cmd, "git status"]
result_os = os.popen(' && '.join(bash_command)).read()
for result in result_os.split('\n'):
    if result.find('modified') != -1:
        prepare_result = result.replace('\tmodified:   ', '')
        print(cmd+"/"+prepare_result)
```

### Вывод скрипта при запуске при тестировании:
`~/test.py`:
```

```~/test.py
/home/sysadm/netology/sysadm-homeworks/HW-3.9/README.md
/home/sysadm/netology/sysadm-homeworks/HW-4.1/README.md
```
`./test.py netology/sysadm-homeworks`:
```./test.py netology/sysadm-homeworks
netology/sysadm-homeworks/HW-3.9/README.md
netology/sysadm-homeworks/HW-4.1/README.md
```

Обрабатывать отдельно случай, если папка не является git репозиторием нет необходимости, т.к. сам git выведет ошибку в терминал:
```
fatal: not a git repository (or any of the parent directories): .git
```

## Обязательная задача 4
1. Наша команда разрабатывает несколько веб-сервисов, доступных по http. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис. Проблема в том, что отдел, занимающийся нашей инфраструктурой очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков. Мы хотим написать скрипт, который опрашивает веб-сервисы, получает их IP, выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>. Также, должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена - оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: `drive.google.com`, `mail.google.com`, `google.com`.

### Решение:
```python
#!/usr/bin/env python3

import socket
import time

Servers = {'drive.google.com':'null', 'mail.google.com':'null', 'google.com':'null'}
while True:
    for dnsname, ipaddr in Servers.items():
        time.sleep(1)
        ip = socket.gethostbyname(dnsname)
        print(dnsname + ' - ' + ip)
        if  (ipaddr != 'null') and (ip != ipaddr):
            print('[ERROR] ' + dnsname + ' IP mismatch: ' + ipaddr + ' ' + ip)
        Servers[dnsname] = ip
```

### Вывод скрипта при запуске при тестировании:
```./iptest.py
drive.google.com - 209.85.233.194
mail.google.com - 74.125.131.19
google.com - 173.194.222.102
drive.google.com - 209.85.233.194
mail.google.com - 74.125.131.19
google.com - 173.194.222.102
drive.google.com - 209.85.233.194
mail.google.com - 74.125.131.19
google.com - 173.194.222.102
drive.google.com - 209.85.233.194
mail.google.com - 64.233.162.18
[ERROR] mail.google.com IP mismatch: 74.125.131.19 64.233.162.18
google.com - 173.194.222.102
^CTraceback (most recent call last):
  File "./iptest.py", line 9, in <module>
    time.sleep(1)
KeyboardInterrupt

```
