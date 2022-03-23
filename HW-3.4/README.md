# Домашняя работа к занятию "3.4. Операционные системы, лекция 2"

1. предусмотрите возможность добавления опций к запускаемому процессу через внешний файл (посмотрите, например, на `systemctl cat cron`),
   ```
   wget https://github.com/prometheus/node_exporter/releases/download/v1.3.1/node_exporter-1.3.1.linux-amd64.tar.gz
   tar xvf node_exporter-1.3.1.linux-amd64.tar.gz
   sudo cp node_exporter-1.3.1.linux-amd64/node_exporter /usr/local/bin
   sudo chown node_exporter:node_exporter /usr/sbin/node_exporter
   rm -rf node_exporter-1.3.1.linux-amd64 node_exporter-1.3.1.linux-amd64.tar.gz
   sudo nano /etc/systemd/system/node_exporter.service
   sudo systemctl daemon-reload
   sudo systemctl start node_exporter
   sudo systemctl status node_exporter
   sudo systemctl enable node_exporter
   sudo reboot
   sudo systemctl status node_exporter 
   ```
   ![Screenshot01](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.4/Screenshot01.png)
2. Посмотреть опции: `node_exporter --help`  
   Метрики: `curl http://localhost:9100/metrics`
   Для базового мониторинга я бы выбрал следующие метрики (соответствующие коллекторы включены по умолчанию):
   **node_disk_io_now** — The number of I/Os currently in progress;
   **node_filesystem_free_bytes** — Filesystem free space in bytes;
   **node_filesystem_size_bytes** — Filesystem size in bytes;
   **node_load1** — 1m load average;
   **node_load15** — 15m load average;
   **node_load5** — 5m load average;
   **node_memory_MemAvailable_bytes** — Memory information field MemAvailable_bytes;
   **node_memory_MemFree_bytes** — Memory information field MemFree_bytes;
   **node_memory_MemTotal_bytes** — Memory information field MemTotal_bytes;
   **node_network_receive_bytes_total** — Network device statistic receive_bytes;
   **node_network_receive_drop_total** — Network device statistic receive_drop;
   **node_network_receive_errs_total** — Network device statistic receive_errs;
   **node_network_transmit_bytes_total** — Network device statistic transmit_bytes;
   **node_network_transmit_drop_total** — Network device statistic transmit_drop;
   **node_network_transmit_errs_total** — Network device statistic transmit_errs.
   Приведите несколько опций, которые вы бы выбрали для базового мониторинга хоста по CPU, памяти, диску и сети.    
3. **cpu** — Total CPU utilization (all cores). 100% here means there is no CPU idle time at all. You can get per core usage at the CPUs section and per application usage at the Applications Monitoring section.  
   **load** — Current system load, i.e. the number of processes using CPU or waiting for system resources (usually CPU and disk). The 3 metrics refer to 1, 5 and 15 minute averages. The system calculates this once every 5 seconds.  
   **disk** — Total Disk I/O, for all physical disks. You can get detailed information about each disk at the Disks section and per application Disk usage at the Applications Monitoring section. Physical are all the disks that are listed in /sys/block, but do not exist in /sys/devices/virtual/block.  
   **ram** — System Random Access Memory (i.e. physical memory) usage.  
   **swap** — System swap memory usage. Swap space is used when the amount of physical memory (RAM) is full. When the system needs more memory resources and the RAM is full, inactive pages in memory are moved to the swap space (usually a disk, a disk partition or a file).  
   **network** — Total bandwidth of all physical network interfaces. This does not include lo, VPNs, network bridges, IFB devices, bond interfaces, etc. Only the bandwidth of physical network interfaces is aggregated. Physical are all the network interfaces that are listed in /proc/net/dev, but do not exist in /sys/devices/virtual/net.  
   **processes** — System processes. Running are the processes in the CPU. Blocked are processes that are willing to enter the CPU, but they cannot, e.g. because they wait for disk activity.  
   **idlejitter** — Idle jitter is calculated by netdata. A thread is spawned that requests to sleep for a few microseconds. When the system wakes it up, it measures how many microseconds have passed. The difference between the requested and the actual duration of the sleep, is the idle jitter. This number is useful in real-time environments, where CPU jitter can affect the quality of the service (like VoIP media gateways).  
   **interrupts** — Total number of CPU interrupts. Check system.interrupts that gives more detail about each interrupt and also the CPUs section where interrupts are analyzed per CPU core.  
   **softirqs** — CPU softirqs in detail. At the CPUs section, softirqs are analyzed per CPU core.  
   **entropy** — Entropy, is a pool of random numbers (/dev/random) that is mainly used in cryptography. If the pool of entropy gets empty, processes requiring random numbers may run a lot slower (it depends on the interface each program uses), waiting for the pool to be replenished. Ideally a system with high entropy demands should have a hardware device for that purpose (TPM is one such device). There are also several software-only options you may install, like haveged, although these are generally useful only in servers.  
   **uptime**  
   Далее следуют более подробные метрики по каждому пункту...  
   ![Screenshot03](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.4/Screenshot03.png)
4. Можно: `[Mon Mar 14 19:25:19 2022] Hypervisor detected: KVM` 
5. По умолчанию значение `nr_open` равно 1048576. Узнать можно так: `cat /proc/sys/fs/nr_open`. `nr_open` задает максимальное количество файловых дескрипторов, открываемых одним процессом. Достичь этого значения не позволит лимит `open files` (`ulimit -n`).
6. В основном терминале запускаем: `sudo unshare -f --pid --mount-proc sleep 1h`.  
   Во втором терминале повышаем права, смотрим PID процесса подключаемся к соответствующему namespace'у и убеждаемся, что запущенный изначально процесс имет **PID 1**, а остальные процессы в этом namespace'е невидимы:
   ![Screenshot06](https://raw.githubusercontent.com/Merlin1979/devops-netology/main/HW-3.4/Screenshot06.png)
7. `:(){ :|:& };:` — это форк-бомба.  
   Автоматической стабилизации помог Механизм "cgroup" (запись в dmesg: `cgroup: fork rejected by pids controller in /user.slice/user-1000.slice/session-1.scope`).  
   По умолчанию действует ограничение на количество процессов, которое может запустить пользователь, в 33% от максимально допустимого для системы (`sysctl kernel.threads-max` — для данной системы `kernel.threads-max = 15190`).  
   Изменить значение для всех пользователей можно в файле `/usr/lib/systemd/system/user-.slice.d/10-defaults.conf`.
