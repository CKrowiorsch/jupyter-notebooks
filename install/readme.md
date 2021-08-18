# Installationen

## Nodeexporter

```bash
# User anlegen
sudo useradd --no-create-home --shell /bin/false node_exporter

# download aktuelle version
wget https://github.com/prometheus/node_exporter/releases/download/v1.2.2/node_exporter-1.2.2.darwin-amd64.tar.gz

# auspacken und in usr local bin
tar xvf node_exporter-1.2.2.darwin-amd64.tar.gz
sudo cp node_exporter-0.15.1.linux-amd64/node_exporter /usr/local/bin
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter

# zusatzmetriken
sudo mkdir -p /prometheus/metrics
sudo chown node_exporter:node_exporter /prometheus/metrics
```

ServiceD Installation

```bash
# serviced
sudo vi /lib/systemd/system/node_exporter.service
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
sudo systemctl status node_exporter
```

Konfiguration

```text
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target 

[Service]
Type=simple
User=node_exporter
Group=node_exporter
ExecStart=/usr/local/bin/node_exporter --collector.textfile.directory=/prometheus/metrics
Restart=always
RestartSec=10s
NotifyAccess=all 

[Install]
WantedBy=multi-user.target
```
