# Домашняя работа к занятию "3.9. Элементы безопасности информационных систем"

1. Bitwarden плагин для браузера работает:  
   ![Screenshot101](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.9/Screenshot101.png)  

   Долгое время использую LastPass:  
   ![Screenshot102](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.9/Screenshot102.png)  
   Хотя для бытовых задач вполне достаточно менеджеров паролей, встроенных в современные браузеры.  

 
2. В Bitwarden включена двухфакторная аутентификация через Google OTP. К сожалению, Google OTP не позволяет скриншот, но по скриншоту из браузера видно, что 2FA включена.  
   ![Screenshot201](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.9/Screenshot201.png)


3. Команды установки `apache` (сессию терминала закрыл до того, как сделал скриншоты, поэтому привожу только команды из `history`) и содержимое конфигурационного файла:
   ```
   sudo apt install apache2
   sudo ufw allow "Apache Full"
   sudo a2enmod ssl
   sudo systemctl restart apache2
   sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/apache-selfsigned.key -out /etc/ssl/certs/apache-selfsigned.crt
   sudo nano /etc/apache2/sites-available/dooh.36p.ru.conf
   sudo mkdir /var/www/dooh.36p.ru
   sudo nano /var/www/dooh.36p.ru/index.html
   sudo a2ensite dooh.36p.ru.conf
   sudo systemctl reload apache2
   sudo apache2ctl configtest
   sudo nano /var/www/dooh.36p.ru/index.html
   ```
   `/etc/apache2/sites-available/dooh.36p.ru.conf`:
   ```
   <VirtualHost *:443>
      ServerName dooh.36p.ru
      DocumentRoot /var/www/dooh.36p.ru

      SSLEngine on
      SSLCertificateFile /etc/ssl/certs/apache-selfsigned.crt
      SSLCertificateKeyFile /etc/ssl/private/apache-selfsigned.key
   </VirtualHost>
   ```
   Браузеры выдают предупреждение, т.к. сертификат самоподписанный:
   ![Screenshot301](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.9/Screenshot301.png)

   Но если перейти на сайт несмотря на предупреждение, видно что и сайт работает, и работает именно через https:
   ![Screenshot302](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.9/Screenshot302.png)

   Сертификат в порядке:  
   ![Screenshot303](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.9/Screenshot303.png)


4. Проверяйем `https://e1.ru`. Найдено 4 уязвимости: `Secure Client-Initiated Renegotiation` - серьезная уязвимость, т.к. такая операция пересогласования отнимает относительно много времени и ресурсов сервера, что позволяет успешно провести DDoS-атаку. Вторая уязвимость позволяет провести MITM-атаку и потенциально получить в открытом виде секреты шифрования, но эта уязвимость помечена только как потенциальная угроза, потому что не представляет опасности для статичных страниц и страниц без секретов. Остальные две уязвимости помечены, как незначительные - третья потому что по умолчанию выбирается более надежный протокол без этой уязвимости,а четвертая, т.к. угроза носит маловероятный характер - требует соблюдения многих условий (например, нахождение в одной сети с атакуемым) и отнимает много времени с негарантированным результатом. 
   ![Screenshot401](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.9/Screenshot401.png)


5. Ubuntu в ВМ Vagrant уже содержит установленный ssh-сервер:
   ![Screenshot501](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.9/Screenshot501.png)

   На скриншоте виден процесс генерации ключевой пары, передача открытого ключа на управляемый сервер и подключение с изпользованием ключа:  
   ![Screenshot502](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.9/Screenshot502.png)

 
6. Файл конфигурации SSH-клиента `~/.ssh/config`:
   ![Screenshot602](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.9/Screenshot602.png)

   Переименование файлов ключей так как задали в `~/.ssh/config` и подключение к серверу по имени, указанному в том же файле:
   ![Screenshot603](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.9/Screenshot603.png)


7. Создаем дамп трафика утилитой tcpdump, сохраняем в pcap-файл:  
   ![Screenshot701](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.9/Screenshot701.png)

   Анализируем сохраненные данные в Wireshark:  
   ![Screenshot702](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.9/Screenshot702.png)
