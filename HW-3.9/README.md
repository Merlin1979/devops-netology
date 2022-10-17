# Домашняя работа к занятию "3.9. Элементы безопасности информационных систем"

1. Установите Bitwarden плагин для браузера. Зарегестрируйтесь и сохраните несколько паролей.
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot101.png)
   ![Screenshot02](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot102.png)
   
2. Установите Google authenticator на мобильный телефон. Настройте вход в Bitwarden акаунт через Google authenticator OTP.
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot201.png)

3. Установите apache2, сгенерируйте самоподписанный сертификат, настройте тестовый сайт для работы по HTTPS.
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot301.png)
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot3021.png)
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot303.png)

4. Проверьте на TLS уязвимости произвольный сайт в интернете (кроме сайтов МВД, ФСБ, МинОбр, НацБанк, РосКосмос, РосАтом, РосНАНО и любых госкомпаний, объектов КИИ, ВПК ... и тому подобное).
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot401.png)

5. Установите на Ubuntu ssh сервер, сгенерируйте новый приватный ключ. Скопируйте свой публичный ключ на другой сервер. Подключитесь к серверу по SSH-ключу.
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot501.png)
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot501.png)
 
6. Переименуйте файлы ключей из задания 5. Настройте файл конфигурации SSH клиента, так чтобы вход на удаленный сервер осуществлялся по имени сервера.
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot601.png)
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot602.png)
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot603.png)

7. Соберите дамп трафика утилитой tcpdump в формате pcap, 100 пакетов. Откройте файл pcap в Wireshark.
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot701.png)
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/blob/main/HW-3.9/Screenshot702.png)