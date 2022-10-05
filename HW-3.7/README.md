# Домашняя работа к занятию "3.7. Компьютерные сети, лекция 2"

1. **В Linux:**  
   `ip -c -br l`
   ```
   eth0             2c:4d:54:d1:01:e1 <BROADCAST,MULTICAST,UP>
   eth1             0a:00:27:00:00:0c <BROADCAST,MULTICAST,UP>
   eth2             00:50:56:c0:00:01 <BROADCAST,MULTICAST,UP>
   eth3             00:50:56:c0:00:08 <BROADCAST,MULTICAST,UP>
   lo               00:00:00:00:00:00 <LOOPBACK,UP>
   eth4             00:00:00:00:00:00 <>
   eth5             00:ff:a0:81:5c:86 <>
   ```
   **В Windows:**  
   `ipconfig /all`  
   `netsh interface show interface`  
   `Get-NetAdapter` — PowerShell
   ```
   Name                      InterfaceDescription                    ifIndex Status       MacAddress             LinkSpeed
   ----                      --------------------                    ------- ------       ----------             ---------
   VMware Network Adapte...8 VMware Virtual Ethernet Adapter for ...      38 Up           00-50-56-C0-00-08       100 Mbps
   OpenVPN TAP-Windows6      TAP-Windows Adapter V9                       14 Disconnected 00-FF-A0-81-5C-86         1 Gbps
   VMware Network Adapte...1 VMware Virtual Ethernet Adapter for ...      37 Up           00-50-56-C0-00-01       100 Mbps
   VirtualBox Host-Only N... VirtualBox Host-Only Ethernet Adapter        12 Up           0A-00-27-00-00-0C         1 Gbps
   OpenVPN Wintun            Wintun Userspace Tunnel                       7 Disconnected                         100 Gbps
   Ethernet                  Realtek PCIe GbE Family Controller            5 Up           2C-4D-54-D1-01-E1         1 Gbps   
   ```
2. Протокол для обмена информацией между соседними устройствами — LLDP. В Linux для работы с этим протоколом используется пакет `lldpd`. В него помимо сервиса `lldpd` входят команды `lldpcli` и `lldpctl`. Например для просмотра соседних устройств используется команда `lldpcli show neighbors`:
   ```lldpcli show neighbors
   -------------------------------------------------------------------------------
   LLDP neighbors:
   -------------------------------------------------------------------------------
   Interface:    enp0s3, via: LLDP
     Chassis:     
       ChassisID:    mac 2c:4d:54:d1:01:e1
     Port:        
       PortID:       mac 2c:4d:54:d1:01:e1
       TTL:          3601
   -------------------------------------------------------------------------------
   ```

3. Для разделения на виртуальные сети используется технология VLAN (IEEE 802.1Q). В Linux для работы с VLAN используется одноименный пакет `vlan`.
   `/etc/network/interfaces`:
   ```
   auto eth0.10
   iface eth0.10 inet static
   address 192.168.0.10
   netmask 255.255.255.0
   vlan-raw-device eth0
   ```

4. **mode=0 (balance-rr)** — последовательно кидает пакеты, с первого по последний интерфейс;  
   **mode=1 (active-backup)** — один из интерфейсов активен, если активный интерфейс выходит из строя (link down и т.д.), другой интерфейс заменяет активный, не требует дополнительной настройки коммутатора;  
   **mode=2 (balance-xor)** — передачи распределяются между интерфейсами на основе формулы ((MAC-адрес источника) XOR (MAC-адрес получателя)) % число интерфейсов — один и тот же интерфейс работает с определённым получателем, режим даёт балансировку нагрузки и отказоустойчивость;  
   **mode=3 (broadcast)** — все пакеты на все интерфейсы;  
   **mode=4 (802.3ad)** — Link Agregation — IEEE 802.3ad, требует от коммутатора настройки;  
   **mode=5 (balance-tlb)** — входящие пакеты принимаются только активным сетевым интерфейсом, исходящий распределяется в зависимости от текущей загрузки каждого интерфейса, не требует настройки коммутатора;  
   **mode=6 (balance-alb)** — тоже самое что 5, только входящий трафик тоже распределяется между интерфейсами, не требует настройки коммутатора, но интерфейсы должны уметь изменять MAC.
   `/etc/network/interfaces`:
   ```
   auto bond0
   iface bond0 inet static
   address 192.168.10.10
   netmask 255.255.255.0    
   gateway 192.168.10.1
   dns-nameservers 192.168.10.1 77.88.8.8
   dns-search domain.local
   slaves eth0 eth1
   bond_mode 0
   bond-miimon 100
   bond_downdelay 200
   bound_updelay 200
   ```


5. В сети с маской /29 6 IP-адресов для хостов (без учета адреса сети и Broadcast-адреса). Из сети с маской /24 можно получить 32 подсети с маской /29.  
   Примеры:  
   `10.10.10.0/29`  
   `10.10.10.8/29`  
   `10.10.10.16/29`  
   `...`  
   `10.10.10.248/29`

6. Частные IP адреса допустимо взять из 100.64.0.0/10. Под необходимые нужды подойдет 100.64.0.0/26 (62 IP-адоеса).

7. **В Linux:**
   `ip neigh`  
   `ip neigh del <IP-адрес> dev <устройство>`, в моем случае `sudo ip neigh del 192.168.1.101 dev enp0s3`  
   `ip neigh flush dev <устройст>`, в моем случае `sudo ip neigh flush dev enp0s3`
   
   **В Windows:**  
   `arp -a`  
   `netsh interface ip delete arpcache`
   `arp -d <IP-адрес>` 
