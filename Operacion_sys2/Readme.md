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
###### ***Добавляем в файл prometheus.yml***
###### ***- job_name: "node_localhost"***
######    ***static_configs:***
######     ***- targets: ["localhost:9100"]***
***
#### root@vagrant:~# vim /etc/systemd/system/prometheus.service
***
###### ***Создаем файл prometheus.service***
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
# 2.
