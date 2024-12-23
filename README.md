# Os-Server-Final-Project-UAS-23.83.0971 (baru)

- Nama:Muhammad Hilmi Zaidan Rafi
- Kelas:23TK01
- Mata Kuliah:OS SERVER dan SISTEM ADMIN

## Daftar Isi
1. [Installasi APACHE2](#1-installasi-apache2)
2. [Installasi GRAFANA](#2-installasi-grafana)

# 1. Installasi APACHE2
### 1. Instalasi
**Langkah 1:Perbarui daftar paket dengan perintah**
```
sudo apt update
```

**Langkah 2:Installasi apache 2 dengan perintah**
```
sudo apt install apache2 -y
```
- samakan dengan foto di bawah

**Langkah 3:Membuat direktori**
```
sudo mkdir /var/www/server1
```
**Langkah 4:Cloning website yang ada di github**
```
git clone https://github.com/username/repository.git
```
- contoh git clone https://github.com/HilmiZaidanRafi/Penjualan-Tiket-Konser-0971.git

**Langkah 5:Konfigurasi Apache2**
```
sudo nano /etc/apache2/sites-available/server1.conf
```
```
<VirtualHost *:80>
  ServerAdmin admin@192.168.100.52
  DocumentRoot /var/www/server1/Penjualan-Tiket-Konser-0971.git
  ServerName 192.168.100.52
  ErrorLog ${APACHE_LOG_DIR}/error.log
  CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

untuk simpan tekan ctrl + o trus enter dan ctrl + x untuk keluar

**Langkah 6:Aktifkan Virtual Host**
```
sudo a2ensite server1.conf
sudo systemctl reload apache2
```
**Langkah 7:Buka Port**
```
sudo ufw allow 80/tcp
sudo ufw enable
sudo ufw status
```

# 2. Installasi GRAFANA
**Langkah 1:Install grafana**
```
sudo apt install -y software-properties-common
sudo add-apt-repository "deb https://packages.grafana.com/oss/deb stable main"
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
sudo apt update
sudo apt install grafana
```

**Langkah 2:Mulai Layanana**
```
sudo systemctl start grafana-server
sudo systemctl enable grafana-server
```

# 3. Installasi PROMETHEUS
**Langkah 1:Install prometheus**
```
sudo apt update
sudo apt install -y prometheus
```

**Langkah 2:Install Node Exporter**
```
sudo apt install -y prometheus-node-exporter
```

**Langkah 3:Install Node Exporter**
```
sudo apt install -y prometheus-node-exporter
```
- ubah pada bagian "job_name"
```
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  # Monitor Apache server
  - job_name: 'apache'
    static_configs:
      - targets: ['localhost:80']
    metrics_path: '/server-status'
    params:
      auto: ['']

  # Monitor Node Exporter
  - job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']

  # Monitor Prometheus itself
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
```

**Langkah 4:Konfigurasi Apache untuk mengekspos metrics**
```
sudo nano /etc/apache2/mods-available/status.conf
```
- ubah pada bagian "Require"
```
<Location "/server-status">
    SetHandler server-status
    Require local
</Location>
```

**Langkah 5:Aktifkan mod_status Apache**
```
sudo a2enmod status
sudo systemctl restart apache2
```

**Langkah 6:Restart Layanan
```
sudo systemctl restart prometheus
```

*Langkah 7:Verifikasi Status Layanan
```
sudo systemctl status prometheus
sudo systemctl status grafana-server
sudo systemctl status prometheus-node-exporter
```
