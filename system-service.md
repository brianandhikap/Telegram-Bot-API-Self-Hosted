# systemd Service – Telegram Bot API

File ini digunakan agar **Telegram Bot API berjalan otomatis** dan restart sendiri jika crash.

---

## 1. Buat File Service

```bash
nano /etc/systemd/system/telegram-bot-api.service
```

---

## 2. Isi File Service

```ini
[Unit]
Description=Telegram Bot API (Self Hosted)
After=network.target

[Service]
Type=simple
User=root
WorkingDirectory=/opt/telegram-bot-api/build
ExecStart=/opt/telegram-bot-api/build/telegram-bot-api \
  --api-id=API_ID_MU \
  --api-hash=API_HASH_MU \
  --http-port=8081 \
  --dir=/opt/tg-bot-data \
  --verbosity=3
Restart=always
RestartSec=5
LimitNOFILE=1048576

[Install]
WantedBy=multi-user.target
```

⚠️ **Ganti `API_ID_MU` dan `API_HASH_MU` sesuai milikmu**

---

## 3. Reload & Enable Service

```bash
systemctl daemon-reexec
systemctl daemon-reload
systemctl enable telegram-bot-api
systemctl start telegram-bot-api
```

---

## 4. Cek Status

```bash
systemctl status telegram-bot-api
```

---

## 5. Log Monitoring

```bash
journalctl -u telegram-bot-api -f
```

---

✅ Telegram Bot API sekarang **auto start + auto restart**.
