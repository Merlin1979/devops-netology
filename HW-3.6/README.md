# Домашняя работа к занятию "3.6. Компьютерные сети, лекция 1"

1. ```
   HTTP/1.1 301 Moved Permanently
   cache-control: no-cache, no-store, must-revalidate
   location: https://stackoverflow.com/questions
   ```
   Код HTTP: 301 — перемещен постоянно. В строке `location` указан новый адрес. Используется в случае, когда запрошенный ресурс был на постоянной основе перемещён в новое месторасположение, и указывающий на то, что текущие ссылки, использующие данный URL, должны быть обновлены.
   В данном случае можно сделать вывод, что данный сайт доступен только по HTTPS, а перенаправление предлагается по аналогичному адресу только по протоколу HTTPS.
2. `Status Code: 307 Internal Redirect`  
   Время загрузки страницы 1,13 с., дольше всего обрабатывался запрос `GET https://stackoverflow.com/` - 355 мс.
   ![Screenshot01](https://github.com/Merlin1979/devops-netology/raw/main/HW-3.6/Screenshot02.png)
3. `curl ifconfig.me/ip`  
   `46.229.98.236`
4. По данным `whois` провайдер "MIRALOGIC-NET" ("LLC KOMTEHCENTR FTTB Customers"), AS12668.
   ```
   % This is the RIPE Database query service.
   % The objects are in RPSL format.
   %
   % The RIPE Database is subject to Terms and Conditions.
   % See http://www.ripe.net/db/support/db-terms-conditions.pdf
   
   % Note: this output has been filtered.
   %       To receive output for a database update, use the "-B" flag.
   
   % Information related to '46.229.98.0 - 46.229.98.255'
   
   % Abuse contact for '46.229.98.0 - 46.229.98.255' is 'noc@itmh.ru'
   
   inetnum:        46.229.98.0 - 46.229.98.255
   netname:        MIRALOGIC-NET
   descr:          LLC KOMTEHCENTR FTTB Customers
   descr:          Yekaterinburg, Russian Federation
   country:        RU
   language:       RU
   geoloc:         56.8519 60.6122
   admin-c:        MRL42-RIPE
   tech-c:         MRL42-RIPE
   status:         ASSIGNED PA
   remarks:        INFRA-AW
   mnt-by:         EXTRIM-MNT
   created:        2021-02-18T08:27:14Z
   last-modified:  2021-11-16T05:25:32Z
   source:         RIPE # Filtered
   
   role:           MIRALOGIC NOC role
   address:        office 211, 46 Sulimova str., Yekaterinburg, Russia
   admin-c:        ASD11-RIPE
   admin-c:        JLJ26-RIPE
   admin-c:        AV1359-RIPE
   tech-c:         MNTR1-RIPE
   tech-c:         VETR-RIPE
   abuse-mailbox:  noc@itmh.ru
   nic-hdl:        MRL42-RIPE
   mnt-by:         EXTRIM-MNT
   created:        2012-06-07T12:30:27Z
   last-modified:  2020-09-08T14:23:03Z
   source:         RIPE # Filtered
   
   % Information related to '46.229.96.0/21AS12668'
   
   route:          46.229.96.0/21
   origin:         AS12668
   mnt-by:         EXTRIM-MNT
   created:        2021-02-18T05:07:21Z
   last-modified:  2021-02-18T05:07:21Z
   source:         RIPE
   
   % This query was served by the RIPE Database Query Service version 1.102.2 (HEREFORD)
   ```
5. Пакет до `8.8.8.8` про ходит чере следующие автономные системы: AS12668 (KOMTEHCENTR), AS8359 (MTS), AS15169 (Google).  
   `traceroute -A 8.8.8.8`
   ```
   traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
    1  _gateway (192.168.1.1) [*]  0.537 ms  0.495 ms  0.478 ms
    2  212.193.21.1-FTTB.planeta.tc (212.193.21.1) [AS49392/AS12668]  2.436 ms  2.533 ms  2.510 ms
    3  be5-4031.sr36-27.ekb.ru.mirasystem.net (92.242.30.102) [AS12668]  3.572 ms  3.797 ms  4.124 ms
    4  212.188.22.34 (212.188.22.34) [AS8359]  4.706 ms  4.484 ms  7.773 ms
    5  asb-cr01-ae9.200.ekt.mts-internet.net (212.188.22.33) [AS8359]  4.394 ms  2.700 ms  4.543 ms
    6  * * zoo-cr03-be1.66.ekt.mts-internet.net (212.188.29.249) [AS8359]  2.976 ms
    7  * * *
    8  * * *
    9  mag9-cr01-be16.77.msk.mts-internet.net (212.188.29.82) [AS8359]  30.934 ms  30.804 ms  30.506 ms
   10  108.170.250.66 (108.170.250.66) [AS15169]  30.669 ms 108.170.250.113 (108.170.250.113) [AS15169]  31.498 ms 108.170.250.66 (108.170.250.66) [AS15169]  30.522 ms
   11  * 142.251.237.154 (142.251.237.154) [AS15169]  45.860 ms 142.251.237.156 (142.251.237.156) [AS15169]  45.828 ms
   12  72.14.238.168 (72.14.238.168) [AS15169]  44.411 ms 209.85.254.20 (209.85.254.20) [AS15169]  45.198 ms  45.308 ms
   13  72.14.236.73 (72.14.236.73) [AS15169]  45.843 ms 142.250.209.35 (142.250.209.35) [AS15169]  47.747 ms 172.253.51.241 (172.253.51.241) [AS15169]  48.044 ms
   14  * * *
   15  * * *
   16  * * *
   17  * * *
   18  * * *
   19  * * *
   20  * * *
   21  * * *
   22  * * *
   23  * dns.google (8.8.8.8) [AS15169]  46.492 ms  43.644 ms
   ```
6. Наибольшая задержка на узле `209.85.255.136` — 76.9 мс.  
   `mtr 8.8.8.8`
   ```
   merlin-VirtualBox (192.168.1.125) -> 8.8.8.8                        2022-03-29T11:51:21+0500
   Change Packet Size: 64
   Size Range: 28-4470, < 0:random.
    Host                                             Loss%   Snt   Last   Avg  Best  Wrst StDev
    1. _gateway                                       0.0%    94    1.0   0.6   0.4   1.0   0.2
    2. 212.193.21.1-FTTB.planeta.tc                   0.0%    94    1.8   2.1   1.3  20.4   2.2
    3. be5-4031.sr36-27.ekb.ru.mirasystem.net         0.0%    94    2.7   2.6   2.1   5.2   0.5
    4. 212.188.22.34                                  0.0%    94    7.4   4.6   2.5  20.0   2.2
    5. asb-cr01-ae9.200.ekt.mts-internet.net          0.0%    94    2.5   2.6   1.9  30.5   3.0
    6. zoo-cr03-be1.66.ekt.mts-internet.net          68.8%    94    2.4   2.5   2.1   2.8   0.2
    7. vish-cr01-be7.66.kaz.mts-internet.net         72.0%    94   18.5  18.6  18.2  19.0   0.2
    8. mag9-cr02-be6.16.msk.mts-internet.net         79.3%    93   30.5  30.6  30.1  31.1   0.3
    9. mag9-cr01-be16.77.msk.mts-internet.net         0.0%    93   30.6  30.6  30.0  34.0   0.5
   10. 108.170.250.130                                0.0%    93   30.9  30.9  30.4  37.9   0.8
   11. 209.85.255.136                                 4.3%    93   46.0  46.0  45.0  76.9   3.4
   12. 108.170.235.64                                 0.0%    93   45.8  46.5  45.4  56.8   2.2
   13. 142.250.209.171                                0.0%    93   44.4  44.3  43.7  49.5   0.8
   14. (waiting for reply)
   15. (waiting for reply)
   16. (waiting for reply)
   17. (waiting for reply)
   18. (waiting for reply)
   19. (waiting for reply)
   20. (waiting for reply)
   21. (waiting for reply)
   22. (waiting for reply)
   23. dns.google                                     0.0%    93   43.2  43.1  42.6  43.6   0.2
   ```
7. Домен обслуживают следующие сервера DNS:
   `dig dns.google NS`
   ```
   ;; ANSWER SECTION:
   dns.google.             163     IN      NS      ns2.zdns.google.
   dns.google.             163     IN      NS      ns3.zdns.google.
   dns.google.             163     IN      NS      ns4.zdns.google.
   dns.google.             163     IN      NS      ns1.zdns.google.
   ```
   A-записи для домена:
   `dig dns.google`
   ```
   ;; ANSWER SECTION:
   dns.google.             300     IN      A       8.8.4.4
   dns.google.             300     IN      A       8.8.8.8
   ```

8. У обоих адресов существует только одна PTR-запись — `dns.google.`  
   `dig -x 8.8.4.4`
   ```
   4.4.8.8.in-addr.arpa.   268     IN      PTR     dns.google.
   ```
   `dig -x 8.8.8.8`
   ```
   8.8.8.8.in-addr.arpa.   35      IN      PTR     dns.google.
   ```