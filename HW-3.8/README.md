# Домашняя работа к занятию "3.8. Компьютерные сети, лекция 3"

1. 
    ```
    telnet route-views.routeviews.org
    Username: rviews
    show ip route 46.229.98.236
    show bgp 46.229.96.0/20
    ```

    ``` show ip route 46.229.98.236
    route-views>show ip route 46.229.98.236
    Routing entry for 46.229.96.0/20
      Known via "bgp 6447", distance 20, metric 0
      Tag 3356, type external
      Last update from 4.68.4.46 1d06h ago
      Routing Descriptor Blocks:
      * 4.68.4.46, from 4.68.4.46, 1d06h ago
          Route metric is 0, traffic share count is 1
          AS Hops 3
          Route tag 3356
          MPLS label: none
    ```
   Маршрута до 46.229.98.236/32 не нашлось, привожу вывод маршрутов до сети, найденной в предыдущем пункте, в которой находится мой маршрутизатор.  
    ``` show bgp 46.229.96.0/20
    route-views>show ip bgp 46.229.96.0/20
    BGP routing table entry for 46.229.96.0/20, version 2452311543
    Paths: (23 available, best #16, table default)
      Not advertised to any peer
      Refresh Epoch 1
      3333 1103 12389 12668
        193.0.0.56 from 193.0.0.56 (193.0.0.56)
          Origin IGP, localpref 100, valid, external
          Community: 12668:0
          path 7FE0D33B25C8 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      7018 3356 12389 12668
        12.0.1.63 from 12.0.1.63 (12.0.1.63)
          Origin IGP, localpref 100, valid, external
          Community: 7018:5000 7018:37232
          path 7FE0F5FDD728 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      3267 12389 12668
        194.85.40.15 from 194.85.40.15 (185.141.126.1)
          Origin IGP, metric 0, localpref 100, valid, external
          path 7FE11113F210 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      57866 3356 12389 12668
        37.139.139.17 from 37.139.139.17 (37.139.139.17)
          Origin IGP, metric 0, localpref 100, valid, external
          Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:903 3356:2065 12668:0 57866:100 65100:3356 65103:1 65104:31
          unknown transitive attribute: flag 0xE0 type 0x20 length 0x30
            value 0000 E20A 0000 0064 0000 0D1C 0000 E20A
                  0000 0065 0000 0064 0000 E20A 0000 0067
                  0000 0001 0000 E20A 0000 0068 0000 001F
    
          path 7FE0D8DF7570 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      4901 6079 8359 12668
        162.250.137.254 from 162.250.137.254 (162.250.137.254)
          Origin IGP, localpref 100, valid, external
          Community: 65000:10100 65000:10300 65000:10400
          path 7FE0DBF76C88 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      8283 1299 12389 12668
        94.142.247.3 from 94.142.247.3 (94.142.247.3)
          Origin IGP, metric 0, localpref 100, valid, external
          Community: 1299:30000 8283:1 8283:101
          unknown transitive attribute: flag 0xE0 type 0x20 length 0x18
            value 0000 205B 0000 0000 0000 0001 0000 205B
                  0000 0005 0000 0001
          path 7FE029432108 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      20130 6939 12389 12668
        140.192.8.16 from 140.192.8.16 (140.192.8.16)
          Origin IGP, localpref 100, valid, external
          path 7FE16B1DF4C8 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      20912 3257 174 12389 12668
        212.66.96.126 from 212.66.96.126 (212.66.96.126)
          Origin IGP, localpref 100, valid, external
          Community: 3257:8070 3257:30155 3257:50001 3257:53900 3257:53902 20912:65004
          path 7FE0E3129B88 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      49788 12552 12389 12668
        91.218.184.60 from 91.218.184.60 (91.218.184.60)
          Origin IGP, localpref 100, valid, external
          Community: 12552:12000 12552:12100 12552:12101 12552:22000
          Extended Community: 0x43:100:1
          path 7FE10F504EF0 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      6939 12389 12668
        64.71.137.241 from 64.71.137.241 (216.218.252.164)
          Origin IGP, localpref 100, valid, external
          path 7FE08D5A1D70 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      53767 174 12389 12668
        162.251.163.2 from 162.251.163.2 (162.251.162.3)
          Origin IGP, localpref 100, valid, external
          Community: 174:21101 174:22005 53767:5000
          path 7FE112AFC050 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      7660 2516 12389 12668
        203.181.248.168 from 203.181.248.168 (203.181.248.168)
          Origin IGP, localpref 100, valid, external
          Community: 2516:1050 7660:9003
          path 7FE0E0ECF2F0 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      101 3356 12389 12668
        209.124.176.223 from 209.124.176.223 (209.124.176.223)
          Origin IGP, localpref 100, valid, external
          Community: 101:20100 101:20110 101:22100 3356:2 3356:22 3356:100 3356:123 3356:501 3356:903 3356:2065 12668:0
          Extended Community: RT:101:22100
          path 7FE0FC58C670 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      3561 3910 3356 12389 12668
        206.24.210.80 from 206.24.210.80 (206.24.210.80)
          Origin IGP, localpref 100, valid, external
          path 7FE13F99EF48 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      701 1273 12389 12668
        137.39.3.55 from 137.39.3.55 (137.39.3.55)
          Origin IGP, localpref 100, valid, external
          path 7FE124126528 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      3356 12389 12668
        4.68.4.46 from 4.68.4.46 (4.69.184.201)
          Origin IGP, metric 0, localpref 100, valid, external, best
          Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:903 3356:2065 12668:0
          path 7FE142F19880 RPKI State not found
          rx pathid: 0, tx pathid: 0x0
      Refresh Epoch 1
      19214 174 12389 12668
        208.74.64.40 from 208.74.64.40 (208.74.64.40)
          Origin IGP, localpref 100, valid, external
          Community: 174:21101 174:22005
          path 7FE146EE88A8 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      1351 6939 12389 12668
        132.198.255.253 from 132.198.255.253 (132.198.255.253)
          Origin IGP, localpref 100, valid, external
          path 7FE07FAB47E0 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      852 3356 12389 12668
        154.11.12.212 from 154.11.12.212 (96.1.209.43)
          Origin IGP, metric 0, localpref 100, valid, external
          path 7FE047501540 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      3549 3356 12389 12668
        208.51.134.254 from 208.51.134.254 (67.16.168.191)
          Origin IGP, metric 0, localpref 100, valid, external
          Community: 3356:2 3356:22 3356:100 3356:123 3356:501 3356:903 3356:2065 3549:2581 3549:30840 12668:0
          path 7FE12C375778 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      3303 12389 12668
        217.192.89.50 from 217.192.89.50 (138.187.128.158)
          Origin IGP, localpref 100, valid, external
          Community: 3303:1004 3303:1006 3303:1030 3303:3056 12668:0
          path 7FE0878E3D90 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 3
      2497 12389 12668
        202.232.0.2 from 202.232.0.2 (58.138.96.254)
          Origin IGP, localpref 100, valid, external
          path 7FE0DEF95E90 RPKI State not found
          rx pathid: 0, tx pathid: 0
      Refresh Epoch 1
      3257 3356 12389 12668
        89.149.178.10 from 89.149.178.10 (213.200.83.26)
          Origin IGP, metric 10, localpref 100, valid, external
          Community: 3257:8794 3257:30043 3257:50001 3257:54900 3257:54901
          pathD:\temp.txt 7FE0915F1190 RPKI State not found
          rx pathid: 0, tx pathid: 0    
    ```

2. В Ubuntu 20.04, которая используется в ВМ Vagrant, для настройки сети используется `Netplan`, но он не поддерживает dummy-интерфейсы. Посчитал, что самым простым решением в данном случае, будет удаление `Netplan`, установка `ifupdown` и настройка сети традиционным способом (`/etc/network/interfaces`). Затем в `/etc/network/interfaces` добавляем следующие строки:
    ``` /etc/network/interfaces
    # Dummy network interface
    auto dummy0
    iface dummy0 inet static
      address 192.168.10.10/32
      pre-up ip link add dummy0 type dummy
      post-up ip route add 192.168.100.0/24 via 10.0.2.15
      post-up ip route add 172.16.100.0/24 via 10.0.2.15
    ```
   После перезапуска сети имеем:
    ``` ip route
    vagrant@vagrant:~$ ip r
    default via 10.0.2.2 dev eth0
    10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15
    172.16.100.0/24 via 10.0.2.15 dev eth0
    192.168.100.0/24 via 10.0.2.15 dev eth0
    ```
3. Ниже приведены открытые порты TCP. В первом случае исходящее соединение на 443-й порт — скачивание файла по протоколу https, во втором случае — входящее подключение на 22-й порт, протокол SSH, по которому осуществляется администрирование ВМ Vagrant. 
    ```
    vagrant@vagrant:~$ ss -nt
    State       Recv-Q       Send-Q               Local Address:Port                 Peer Address:Port        Process
    ESTAB       40041        0                        10.0.2.15:34272               87.250.250.50:443
    ESTAB       0            0                        10.0.2.15:22                       10.0.2.2:59922
    ```
4. Ниже приведены открытые UDP-сокеты. Первая запись — это DNS-ресолвер, вторая — используется для загрузки по сети при помощи протокола BOOTP?
    ```
    vagrant@vagrant:~$ ss -ua
    State        Recv-Q       Send-Q               Local Address:Port                 Peer Address:Port       Process
    UNCONN       0            0                    127.0.0.53%lo:domain                    0.0.0.0:*
    UNCONN       0            0                          0.0.0.0:bootpc                    0.0.0.0:*
    ```

5. Схема домашней сети уровня L3 ![L3-scheme](https://github.com/Merlin1979/devops-netology/blob/main/HW-3.8/L3-sheme.png) 
