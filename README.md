# 🤖 TELEGRAM USERBOT DENGAN GEMINI 3.1 AI

## 📌 OVERVIEW

Bot ini adalah **Userbot Telegram Cerdas** yang:
- ✅ **MENAMPILKAN SEMUA CHAT** dari grup dalam real-time
- ✅ Membalas **1-3 pesan random** dengan delay 30-45 detik
- ✅ **REST 1:50 - 2:20** setelah balas (untuk humanize)
- ✅ **BUKA PERCAKAPAN** jika grup sepi > 60 detik
- ✅ Menggunakan **Gemini 3.1 AI** untuk respons natural
- ✅ **MUDAH DIJALANKAN DI TERMUX** (single file)

---

## 🚀 SETUP TERMUX

### 1. Install Python & Dependencies
```bash
pkg update && pkg upgrade
pkg install python git
```

### 2. Clone Repository
```bash
git clone https://github.com/fitrisaja726-arch/telegram-userbot.git
cd telegram-userbot
```

### 3. Install Requirements
```bash
pip install -r requirements.txt
```

### 4. Setup Configuration (.env)
```bash
cp .env.example .env
nano .env
```

**Isi .env dengan:**
```env
TELEGRAM_API_ID=1234567
TELEGRAM_API_HASH=your_api_hash
GEMINI_API_KEY=your_gemini_key
TARGET_GROUP=interlinkIDchat
INDONESIA_TOPIC_ID=26251
```

**Untuk mendapatkan API credentials:**
- Telegram API: https://my.telegram.org/
- Gemini API Key: https://aistudio.google.com/app/apikey

### 5. Run Bot
```bash
python bot.py
```

---

## 📊 FLOW BOT - STEP BY STEP

### Skenario: 5 pesan masuk dalam waktu singkat

```
[15:00:00] BOT START ✅

[15:00:05] 💬 MESSAGE 1 - Budi: "Halo bro"
[15:00:05] [DISPLAY] Budi: Halo bro
[15:00:05] 📦 Buffer: 1 messages

[15:00:10] 💬 MESSAGE 2 - Andi: "Apa kabar?"
[15:00:10] [DISPLAY] Andi: Apa kabar?
[15:00:10] 📦 Buffer: 2 messages

[15:00:15] 💬 MESSAGE 3 - Citra: "Sepi yah"
[15:00:15] [DISPLAY] Citra: Sepi yah
[15:00:15] 📦 Buffer: 3 messages
[15:00:15] ⚠️ Buffer >= MIN_REPLY, START REPLY SEQUENCE!

════════════════════════════════════════════════════════════════════════════════
🤖 REPLYING TO 2 MESSAGES (Random 1-3)
════════════════════════════════════════════════════════════════════════════════

[15:00:15] Pilih random 2 messages dari buffer
  → Selected: Budi & Citra

[15:00:15] 🤖 REPLY #1 - Budi: "Halo bro"
[15:00:15]    └─ 🤔 Thinking... (38s) [typing indicator on]
[15:00:53]    └─ ✅ [REPLY] Halo juga bro! 👋

[15:00:53] 🤖 REPLY #2 - Citra: "Sepi yah"
[15:00:53]    └─ 🤔 Thinking... (35s) [typing indicator on]
[15:01:28]    └─ ✅ [REPLY] Iya bro, gas ngobrol aja

════════════════════════════════════════════════════════════════════════════════
✅ ALL REPLIES SENT
════════════════════════════════════════════════════════════════════════════════

[15:01:28] ⏸️ BOT RESTING for 1m 58s
  (Sleeping 118 detik untuk terlihat human)

[15:03:26] 🌅 BOT WOKE UP - Ready for next batch!

[15:03:26] Check: Grup sepi > 60s? YES!
  [SMART OPEN] Woi pada ngapain nih? Sepi banget

[15:03:30] 💬 MESSAGE 4 - Doni: "Wkwk iya"
[15:03:30] [DISPLAY] Doni: Wkwk iya
[15:03:30] 📦 Buffer: 2 messages (Andi punya + ini)

[15:03:35] 💬 MESSAGE 5 - Eka: "Gas lanjut"
[15:03:35] [DISPLAY] Eka: Gas lanjut
[15:03:35] 📦 Buffer: 3 messages
[15:03:35] ⚠️ TRIGGER REPLY SEQUENCE AGAIN...
(Cycle repeats)
```

---

## ⚙️ KONFIGURASI BEHAVIOR

Edit di `.env`:

```env
# Jumlah pesan yang dibalas sebelum REST (1-3)
MIN_MESSAGES_REPLY=1      # Minimal 1
MAX_MESSAGES_REPLY=3      # Maksimal 3

# Durasi REST dalam detik (1:50-2:20 = 110-140)
REST_DURATION_MIN=110     # 1 menit 50 detik
REST_DURATION_MAX=140     # 2 menit 20 detik

# Delay antar reply (30-45 detik)
REPLY_DELAY_MIN=30        # Minimum 30s
REPLY_DELAY_MAX=45        # Maximum 45s

# Trigger smart open jika sepi (detik)
SILENCE_THRESHOLD=60      # > 60 detik silent = buka obrolan
```

---

## 🔍 PENJELASAN DETIL

### 1. Message Display (SEMUA CHAT TERLIHAT)
```
[15:00:05] Budi: Halo bro
   └─ 📦 Buffer: 1 messages
```
**Setiap pesan masuk LANGSUNG ditampilkan** ke console. Tidak ada yang terlewat!

### 2. Buffer System
```
Pesan 1 → Buffer
Pesan 2 → Buffer
Pesan 3 → Buffer (≥ MIN_REPLY, trigger reply)
```

**Buffer** menyimpan semua pesan yang belum dibalas. Ketika jumlah ≥ MIN_REPLY, bot mulai membalas.

### 3. Random Reply Selection
```
Pesan di buffer: [Msg1, Msg2, Msg3]
Random count: 2 (antara MIN=1 dan MAX=3)
Selected: [Msg1, Msg3]
Remaining: [Msg2]
```

Bot **memilih random** berapa pesan yang akan dibalas, ini menghindari deteksi bot.

### 4. Thinking Delay (30-45s)
```
[15:00:15] 🤔 Thinking... (38s)
[Bot shows typing indicator]
[15:00:53] Send reply
```

Bot menampilkan typing indicator selama random 30-45 detik. Terlihat seperti human yang sedang mengetik!

### 5. Rest Period (110-140s = 1:50 - 2:20)
```
[15:01:28] ⏸️ BOT RESTING for 1m 58s
[Sleep/rest selama 118 detik]
[15:03:26] 🌅 BOT WOKE UP
```

**PENTING**: Rest ini membuat bot terlihat human. Moderator akan suspicious jika bot terus balas langsung tanpa istirahat!

### 6. Smart Opening
```
[15:03:26] Check: Grup sepi > 60s?
YES → [SMART OPEN] Woi pada ngapain nih? Sepi banget
NO → Continue monitoring
```

Jika grup sepi > 60 detik, bot automatically send opening message untuk memulai percakapan.

---

## 🤖 AI GENERATION

Bot menggunakan **Gemini 3.1 Flash Lite** dengan:

```python
model='gemini-3.1-flash-lite'  # ⚡ Fast & smart
temperature=0.88               # Kreativitas tinggi (0-1)
max_output_tokens=75           # Respons pendek & natural
```

**System Prompts** (5 variasi):
```
1. "Kamu temen gaul. Balas pendek, santai, nyambung sama obrolan."
2. "Jadi temen yang fun dan natural. Jangan formal atau membosankan."
3. "Respond seperti teman yang lagi santai di grup. Keep it real."
4. "Balas dengan energi dan sedikit humor. Jangan terlalu panjang."
5. "Ngobrol seperti teman biasa. Santai, natural, dan straightforward."
```

**Kenapa multiple prompts?** Jika selalu sama, moderator/bot detector bisa identify pola bot!

---

## 📝 LOGGING

Semua aktivitas di-log ke `logs/userbot.log`:

```
2024-07-13 15:00:05 - INFO - Bot starting
2024-07-13 15:00:15 - INFO - Reply #1/2 sent
2024-07-13 15:00:48 - INFO - Bot resting for 118 seconds
2024-07-13 15:01:28 - INFO - Reply #2/2 sent
2024-07-13 15:03:26 - INFO - Smart opening sent
```

---

## 🛡️ SECURITY

- ✅ **Credentials di .env** (tidak hardcoded)
- ✅ **Skip deep replies** (hanya balas top-level)
- ✅ **Skip keywords** (admin, moderator, warning, dll)
- ✅ **Graceful shutdown** (Ctrl+C = clean exit)
- ✅ **Error handling** (jangan crash)

---

## 🔧 TROUBLESHOOTING

### Bot tidak konek ke Telegram
```
❌ Error: Connection failed
```
**Solusi:**
- Check internet connection
- Verify API_ID dan API_HASH di .env
- Pastikan tidak ada proxy/firewall blocking

### Gemini API error
```
❌ Error: Invalid API key
```
**Solusi:**
- Get new API key di https://aistudio.google.com/app/apikey
- Ensure API sudah enable
- Update di .env dengan key baru

### Bot tidak balas pesan
```
Pesan masuk tapi tidak ada reply
```
**Solusi:**
- Check buffer size: `📦 Buffer: X messages`
- Verify MIN_MESSAGES_REPLY setting
- Check logs: `cat logs/userbot.log`

---

## ⚡ QUICK START

```bash
# Clone
git clone https://github.com/fitrisaja726-arch/telegram-userbot.git
cd telegram-userbot

# Setup
pip install -r requirements.txt
cp .env.example .env
nano .env  # Isi credentials

# Run
python bot.py
```

---

**Made with ❤️ for Telegram automation**
