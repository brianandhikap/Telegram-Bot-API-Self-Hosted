# Telegram Bot API (Self-Hosted)

Dokumentasi ini menjelaskan cara **install, build, dan menjalankan Telegram Bot API versi self-hosted** di VPS.

---

## 📌 1. Config Akun Telegram

1. Buka:

   ```
   https://my.telegram.org
   ```
2. Login menggunakan akun Telegram
3. Pilih **API development tools**
4. Simpan data berikut:

   * **API ID**
   * **API HASH**
5. Jika belum ada, silakan buat terlebih dahulu

---

## 🤖 2. Config Bot Telegram

1. Buat bot melalui **@BotFather** di Telegram
2. Ikuti instruksi sampai selesai
3. Simpan **BOT TOKEN** yang diberikan

---

## 🛠️ 3. Install Telegram Bot API (Self-Hosted)

### 3.1 Install Dependency

```bash
apt update

apt install -y \
  git cmake g++ \
  build-essential \
  zlib1g-dev \
  libssl-dev \
  libreadline-dev \
  gperf
```

---

### 3.2 Clone Repository

```bash
cd /opt
git clone --recursive https://github.com/tdlib/telegram-bot-api.git
cd telegram-bot-api/
git submodule update --init --recursive
```

---

### 3.3 Cek File Build

Pastikan file berikut ada:

```bash
ls td/CMakeLists.txt
```

Jika ada → **lanjut build**

---

### 3.4 Build Telegram Bot API

```bash
mkdir build && cd build
cmake ..
cmake --build . --target telegram-bot-api -j$(nproc)
```

---

### 3.5 Buat Folder Data

```bash
mkdir -p /opt/tg-bot-data
chmod 755 /opt/tg-bot-data
```

Opsional (lebih aman):

```bash
chown -R root:root /opt/tg-bot-data
```

---

## 🚀 4. Menjalankan Telegram Bot API

```bash
cd /opt/telegram-bot-api/build

./telegram-bot-api \
  --api-id=API_ID_MU \
  --api-hash=API_HASH_MU \
  --http-port=8081 \
  --dir=/opt/tg-bot-data \
  --verbosity=3
```

---

## 🧪 5. Test Bot API

### 5.1 Test dari Local

```bash
curl http://127.0.0.1:8081/bot<BOT_TOKEN_DARI_BOTFATHER>/getMe
```

Jika response mengandung:

```json
"ok": true
```

✅ Berarti bot API berjalan normal

---

### 5.2 Akses dari Public

> ⚠️ **WAJIB VPS dengan IP Public**

```text
http://IP_PUBLIC_VPS:8081/bot<BOT_TOKEN_DARI_BOTFATHER>/getMe
```

---

## 📦 6. Contoh Usage

* Local:

```
http://127.0.0.1:8081/bot<TOKEN>/getMe
```

* Public:

```
http://IP_PUBLIC_VPS:8081/bot<TOKEN>/getMe
```

---

## 📖 Catatan

* Port default menggunakan **8081** (bebas diganti)
* Pastikan firewall VPS membuka port yang digunakan
* Cocok untuk kebutuhan **bot skala besar / high traffic**

---

🔥 Selesai. Telegram Bot API self-hosted siap digunakan.
