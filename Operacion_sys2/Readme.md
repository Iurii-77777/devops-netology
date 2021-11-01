# 1.
## **Устанавливаем Node_exporter:**
#### vagrant@vagrant:~$ sudo useradd --no-create-home --shell /bin/false node_exporter
#### vagrant@vagrant:~$ wget https://github.com/prometheus/node_exporter/releases/download/v1.2.2/node_exporter-1.2.2.linux-amd64.tar.gz
#### vagrant@vagrant:~$ tar -xvzf node_exporter-1.2.2.linux-amd64.tar.gz
###### ***node_exporter-1.2.2.linux-amd64/***
###### ***node_exporter-1.2.2.linux-amd64/LICENSE***
###### ***node_exporter-1.2.2.linux-amd64/NOTICE***
#### vagrant@vagrant:~$ sudo cp node_exporter-1.2.2.linux-amd64/node_exporter /usr/local/bin/
#### vagrant@vagrant:~$ sudo -i
#### root@vagrant:~# touch node_exporter.service
#### root@vagrant:~# vim node_exporter.service
***
###### ***[Unit]***
###### ***Description=Node Exporter***
###### ***Wants=network-online.target***
###### ***After=network-online.target***
###### 
###### ***[Service]***
###### ***User=node_exporter***
###### ***Group=node_exporter***
###### ***ExecStart=/usr/local/bin/node_exporter***
###### 
###### ***[Install]***
###### ***WantedBy=default.target***
***
#### root@vagrant:~# sudo systemctl daemon-reload
#### root@vagrant:~# sudo systemctl start node_exporter
#### root@vagrant:~# sudo systemctl status node_exporter
######  ***node_exporter.service - Node Exporter***
######    ***Loaded: loaded (/etc/systemd/system/node_exporter.service; disabled; vendor preset: enabled)***
######    ***Active: active (running) since Sun 2021-10-31 10:17:21 UTC; 7s ago***
######  ***Main PID: 1362 (node_exporter)***
######     ***Tasks: 4 (limit: 2279)***
######    ***Memory: 2.2M***
######    ***CGroup: /system.slice/node_exporter.service***
######            ***└─1362 /usr/local/bin/node_exporter***        
#### root@vagrant:~# sudo systemctl enable node_exporter
#### root@vagrant:# curl 'localhost:9100/metrics'  #(_проверяем что метрики отдаются_)
## **Устанавливаем Prometheus:**
#### root@vagrant:~# sudo useradd --no-create-home --shell /bin/false prometheus
#### root@vagrant:~# wget https://github.com/prometheus/prometheus/releases/download/v2.30.3/prometheus-2.30.3.linux-amd64.tar.gz
#### root@vagrant:~# tar -xvzf prometheus-2.30.3.linux-amd64.tar.gz
###### ***prometheus-2.30.3.linux-amd64/***
###### ***prometheus-2.30.3.linux-amd64/consoles/***
###### ***prometheus-2.30.3.linux-amd64/consoles/index.html.example***
###### ***prometheus-2.30.3.linux-amd64/consoles/node-cpu.html***
###### ***prometheus-2.30.3.linux-amd64/consoles/node-disk.html***
###### ***prometheus-2.30.3.linux-amd64/consoles/node-overview.html***
###### ***prometheus-2.30.3.linux-amd64/consoles/node.html***
###### ***prometheus-2.30.3.linux-amd64/consoles/prometheus-overview.html***
###### ***prometheus-2.30.3.linux-amd64/consoles/prometheus.html***
###### ***prometheus-2.30.3.linux-amd64/console_libraries/***
###### ***prometheus-2.30.3.linux-amd64/console_libraries/menu.lib***
###### ***prometheus-2.30.3.linux-amd64/console_libraries/prom.lib***
###### ***prometheus-2.30.3.linux-amd64/prometheus.yml***
###### ***prometheus-2.30.3.linux-amd64/LICENSE***
###### ***prometheus-2.30.3.linux-amd64/NOTICE***
###### ***prometheus-2.30.3.linux-amd64/prometheus***
###### ***prometheus-2.30.3.linux-amd64/promtool***
#### root@vagrant:~# sudo cp prometheus-2.30.3.linux-amd64/prometheus /usr/local/bin/
#### root@vagrant:~# sudo cp prometheus-2.30.3.linux-amd64/promtool /usr/local/bin/
#### root@vagrant:~# sudo mkdir /etc/prometheus
#### root@vagrant:~# sudo cp -r prometheus-2.30.3.linux-amd64/consoles/ \
#### > /etc/prometheus/consoles
#### root@vagrant:~# sudo cp -r prometheus-2.30.3.linux-amd64/console_libraries/ \
#### > /etc/prometheus/console_libraries
#### root@vagrant:~# sudo cp -r prometheus-2.30.3.linux-amd64/prometheus.yml \
#### > /etc/prometheus/
#### root@vagrant:~# sudo chown -R prometheus:prometheus /etc/prometheus
#### root@vagrant:~# sudo mkdir /var/lib/prometheus
#### root@vagrant:~# sudo chown prometheus:prometheus /var/lib/prometheus
#### root@vagrant:~# vim /etc/prometheus/prometheus.yml
***
## **Добавляем параметры в файл prometheus.yml**
###### ***- job_name: "node_localhost"***
######    ***static_configs:***
######     ***- targets: ["localhost:9100"]***
***
#### root@vagrant:~# vim /etc/systemd/system/prometheus.service
***
## **Создаем файл prometheus.service**
###### ***[Unit]***
###### ***Description=Prometheus***
###### ***Wants=network-online.target***
###### ***After=network-online.target***
###### 
###### ***[Service]***
###### ***User=prometheus***
###### ***Group=prometheus***
###### ExecStart=/usr/local/bin/prometheus \
######     --config.file /etc/prometheus/prometheus.yml \
######     --storage.tsdb.path /var/lib/prometheus/ \
######     --web.console.templates=/etc/prometheus/consoles \
######     --web.console.libraries=/etc/prometheus/console_libraries
###### 
###### ***[Install]***
###### ***WantedBy=default.target***
***
#### root@vagrant:~# sudo systemctl daemon-reload
#### root@vagrant:~# sudo systemctl start prometheus
#### root@vagrant:~# sudo systemctl status prometheus
######  ***prometheus.service - Prometheus***
######      ***Loaded: loaded (/etc/systemd/system/prometheus.service; disabled; vendor preset: enabled)***
######      ***Active: active (running) since Sun 2021-10-31 10:31:15 UTC; 8s ago***
######      ***Main PID: 1470 (prometheus)***
######      ***Tasks: 7 (limit: 2279)***
######      ***Memory: 18.2M***
######      ***CGroup: /system.slice/prometheus.service***
######              ***└─1470 /usr/local/bin/prometheus --config.file /etc/prometheus/prometheus.yml --storage.tsdb.path /var/lib>***   
#### root@vagrant:~# sudo systemctl enable prometheus
#### root@vagrant:~# curl 'localhost:9090/metrics' #(_Проверяем, что Prometheus отдает свои собственные метрики_)
***
***
## **Далее проверяем перезапуск сервиса node_explorer**
#### root@vagrant:~# ps -e | grep node_exporter
######   1362 ?        00:00:24 node_exporter
#### root@vagrant:~# systemctl stop node_exporter
#### root@vagrant:~# ps -e | grep node_exporter
#### root@vagrant:~# systemctl start node_exporter
#### root@vagrant:~# ps -e | grep node_exporter
######   2062 ?        00:00:00 node_exporter
## **Проверяем назначение переменной**
#### root@vagrant:~# sudo cat /proc/2062/environ
###### LANG=en_US.UTF- 8LANGUAGE=en_US:PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/binHOME=/home/node_exporterLOGNAME=node_exporterUSER=node_exporterINVOCATION_ID=eb4f97840d51430885e917272f4aa731JOURNAL_STREAM=9:37476 
# 2.
## **CPU:**
#### root@vagrant:~# curl 'localhost:9100/metrics' | grep cpu
###### go_memstats_gc_cpu_fraction
###### node_cpu_guest_seconds_total
###### node_cpu_seconds_total
###### node_memory_Percpu_bytes
###### node_pressure_cpu_waiting_seconds_total
###### node_schedstat_running_seconds_total
###### node_schedstat_timeslices_total
###### node_schedstat_waiting_seconds_total
###### node_scrape_collector_duration_seconds
###### node_scrape_collector_success
###### node_softnet_dropped_total
###### node_softnet_processed_total
###### node_softnet_times_squeezed_total
###### process_cpu_seconds_total
## **Memory:**
#### root@vagrant:~# curl 'localhost:9100/metrics' | grep memory
###### node_memory_Active_anon_bytes
###### node_memory_Active_bytes
###### node_memory_Active_file_bytes
###### node_memory_Cached_bytes #(и так далее)
## **Disk:**
#### root@vagrant:~# curl 'localhost:9100/metrics' | grep disk
###### node_disk_io_time_seconds_total{device="dm-0"} 50.368
###### node_disk_read_bytes_total{device="dm-0"} 3.25788672e+08
###### node_disk_written_bytes_total{device="dm-0"} 1.09955072e+09 #(и так далее)
## **Memory:**
#### root@vagrant:~# curl 'localhost:9100/metrics' | grep network
###### node_network_info
###### node_network_mtu_bytes{device="eth0"} #(и так далее)
# 3.
## **Настройки vagrantfile:**
***
###### Vagrant.configure("2") do |config|
###### 	config.vm.box = "bento/ubuntu-20.04"
###### 	config.vm.network "forwarded_port", guest: 19999, host: 19999
###### 	config.vm.provider "virtualbox" do |vb|
###### 		vb.memory = "2048"
######   		vb.cpus = "2"
###### 	end
######  end
***
## **Теперь могу войти в web-панель netdata на хостовой машине через googlechrome по адресу http://localhost:19999/#menu_system_submenu_cpu;theme=slate**
## **Видно метрики которые указывал в задании №2. Отображение метрик более легко читаемо, таблицы и графики зрительно легко воспринимаются**
# 4.
## **С помощью команды dmesg мы видим, что ОС осознают что загружена на виртуальной машине. Если правильно грепнуть, то можем увидеть это в логах.**
#### vagrant@vagrant:~$ dmesg | grep virtual
###### [    0.002115] CPU MTRRs all blank - virtualized system.
###### [    0.073176] Booting paravirtualized kernel on KVM
###### [    3.103783] systemd[1]: Detected virtualization oracle.
# 5.
## **Есть "системное ограничение", задаваемое через sysctl, на лимит количества открытых дескрипторов файла:**
#### vagrant@vagrant:~$ /sbin/sysctl -n fs.nr_open
###### 1048576
## **Следующее место задания ограничений, на этот раз постоянных — это /etc/security/limits.conf и каталог /etc/security/limits.d/, ограничение называется nofile. Там задаются ограничения на отдельных пользователей или группы, применяемые на всю сессию данного пользователя, или всех пользователей определенной группы.**
## **Макс.предел ОС можно посмотреть так :**
#### vagrant@vagrant:~$ cat /proc/sys/fs/file-max
###### 9223372036854775807
# 6.
## **Запускаем команду в основном терминале:**
#### root@vagrant:~# ps aux
###### USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
###### root           1  0.0  0.5 101868 11420 ?        Ss   10:54   0:00 /sbin/init
###### root           2  0.0  0.0      0     0 ?        S    10:54   0:00 [kthreadd]
###### root           3  0.0  0.0      0     0 ?        I<   10:54   0:00 [rcu_gp]
###### root           4  0.0  0.0      0     0 ?        I<   10:54   0:00 [rcu_par_gp]
###### root           6  0.0  0.0      0     0 ?        I<   10:54   0:00 [kworker/0:0H-kblockd]
###### root           9  0.0  0.0      0     0 ?        I<   10:54   0:00 [mm_percpu_wq]
###### root          10  0.0  0.0      0     0 ?        S    10:54   0:00 [ksoftirqd/0]
###### root          11  0.0  0.0      0     0 ?        I    10:54   0:01 [rcu_sched]
###### и т.д. последний pid 9038
## **Создаём изолированныей namespace в дополнительном терминале и запускаем команду sleep:**
#### root@vagrant:/home/vagrant# unshare -f --pid --mount-proc /bin/bash
#### root@vagrant:/home/vagrant# ps aux
###### USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
###### root           1  0.0  0.1   9836  3932 pts/2    S    13:46   0:00 /bin/bash
###### root           8  0.0  0.1  11492  3304 pts/2    R+   13:46   0:00 ps aux
#### root@vagrant:/home/vagrant# sleep 1h
## **Теперь из основного терминала смотрим на запущённые команды в изолированном namespace**
###### root        9078  0.0  0.2   9836  4076 pts/2    Ss   13:46   0:00 /bin/bash
###### root        9085  0.0  0.0   8080   592 pts/2    S    13:46   0:00 unshare -f --pid --mount-proc /bin/bash
###### root        9086  0.0  0.1   9836  3932 pts/2    S+   13:46   0:00 /bin/bash
#### root@vagrant:~# nsenter --target 9086 --pid --mount
#### root@vagrant:/# ps aux
###### USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
###### root           1  0.0  0.1   9836  3996 pts/2    S    13:46   0:00 /bin/bash
###### root           9  0.0  0.0   8076   528 pts/2    S+   13:47   0:00 sleep 1h
###### root          10  0.0  0.1   9836  3896 pts/1    S    13:49   0:00 -bash
###### root          19  0.0  0.1  11492  3296 pts/1    R+   13:49   0:00 ps aux
# 7.
