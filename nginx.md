# Nginx Reverse Proxy + HTTPS (Telegram Bot API)

Dokumentasi ini menjelaskan cara **mengamankan Telegram Bot API** menggunakan **Nginx + HTTPS (Let's Encrypt)**.

---

## 1. Install Nginx & Certbot

```bash
apt update
apt install -y nginx certbot python3-certbot-nginx
```

---

## 2. Konfigurasi Nginx

Buat file konfigurasi baru:

```bash
nano /etc/nginx/sites-available/telegram-bot-api
```

Isi konfigurasi:

```nginx
server {
    listen 80;
    server_name bot.domainmu.com;

    location / {
        proxy_pass http://127.0.0.1:8081;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_connect_timeout 60s;
        proxy_send_timeout 60s;
        proxy_read_timeout 60s;
    }
}
```

⚠️ Ganti `bot.domainmu.com` dengan domain milikmu

---

## 3. Aktifkan Config

```bash
ln -s /etc/nginx/sites-available/telegram-bot-api /etc/nginx/sites-enabled/
nginx -t
systemctl reload nginx
```

---

## 4. Pasang HTTPS (Let's Encrypt)

```bash
certbot --nginx -d bot.domainmu.com
```

Ikuti instruksi sampai selesai.

---

## 5. Auto Renew SSL

Certbot otomatis membuat cron job. Cek dengan:

```bash
certbot renew --dry-run
```

---

## 6. Cara Akses Bot API

Gunakan URL berikut:

```text
https://bot.domainmu.com/bot<BOT_TOKEN>/getMe
```

---

## 7. Tips Keamanan (Opsional)

* Jangan expose port `8081` ke publik
* Batasi akses dengan firewall
* Gunakan domain khusus bot API

---

✅ Telegram Bot API sekarang **aman, HTTPS, dan production ready**.
