# Panduan Instalasi MailCow Mail Server

## 1. Konfigurasi DNS

Tambahkan record berikut pada DNS domain Anda:

| Jenis   | Nama           | Nilai                        |
|---------|----------------|------------------------------|
| A       | mail           | 1.2.3.4 (ganti dengan IP Anda) |
| CNAME   | autodiscover   | mail.yourdomain.com          |
| CNAME   | autoconfig     | mail.yourdomain.com          |
| MX      | @              | mail.yourdomain.com (prioritas 10) |

---

## 2. Install Docker dan Docker Compose

Jalankan perintah berikut satu per satu:

```bash
sudo apt-get update -y
```

```bash
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common -y
```

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

```bash
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

```bash
sudo apt-get update -y
```

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
```

```bash
sudo docker run hello-world
```

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

```bash
docker-compose --version
```

---

## 3. Install MailCow

Jalankan perintah berikut satu per satu:

```bash
sudo hostnamectl set-hostname mail.yourdomain.com
```

```bash
sudo apt install git -y
```

```bash
cd /opt
```

```bash
sudo git clone https://github.com/mailcow/mailcow-dockerized
```

```bash
cd mailcow-dockerized
```

```bash
sudo ./generate_config.sh
```

```bash
sudo docker-compose pull
```

```bash
sudo docker-compose up -d
```

---

## 4. Buka Firewall

```bash
sudo ufw allow 25,80,443,110,143,465,587,993,995/tcp
```

---

## 5. Konfigurasi SPF dan DMARC

Tambahkan record berikut pada DNS:

**SPF Record**
```
TXT   @      "v=spf1 mx a -all"
```

**DMARC Record**
```
TXT   _dmarc "v=DMARC1; p=none; fo=1; rua=mailto:dmarc@yourdomain.com; ruf=mailto:dmarc@yourdomain.com"
```

---

**Catatan:**  
- Ganti `yourdomain.com` dan `1.2.3.4` dengan domain serta IP server Anda.
- Jalankan semua perintah dengan hak akses root atau gunakan `sudo`.
