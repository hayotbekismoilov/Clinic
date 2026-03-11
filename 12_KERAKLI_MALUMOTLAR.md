# 12. Buyurtmachidan Kerakli Ma'lumotlar va Konfiguratsiyalar

> **Hujjat maqsadi:** Loyihani ishga tushirish uchun buyurtmachi (klinika) tomonidan taqdim etilishi zarur bo'lgan **barcha** ma'lumotlar, kalitlar, tokenlar va kontentning to'liq ro'yxati.  
> **Versiya:** 1.0  
> **Yaratilgan sana:** 2026-03-11  
> **Holati:** ⏳ Yig'ilmoqda  

---

## 📋 Umumiy Holat Jadvali

| # | Kategoriya | Holati | Muhimlik |
|---|-----------|--------|----------|
| 1 | [Klinika ma'lumotlari](#1-klinika-umumiy-malumotlari) | ⬜ Kutilmoqda | 🔴 Kritik |
| 2 | [Brending va dizayn](#2-brending-va-dizayn-materiallari) | ⬜ Kutilmoqda | 🔴 Kritik |
| 3 | [Domen va hosting](#3-domen-va-hosting) | ⬜ Kutilmoqda | 🔴 Kritik |
| 4 | [SMS provayder](#4-sms-provayder-credentials) | ⬜ Kutilmoqda | 🔴 Kritik |
| 5 | [To'lov tizimlari](#5-tolov-tizimlari-click--payme) | ⬜ Kutilmoqda | 🔴 Kritik |
| 6 | [Telegram Bot](#6-telegram-bot) | ⬜ Kutilmoqda | 🔴 Kritik |
| 7 | [AI / OpenAI](#7-ai--openai-api) | ⬜ Kutilmoqda | 🟠 Yuqori |
| 8 | [Google Maps](#8-google-xarita-api) | ⬜ Kutilmoqda | 🟡 O'rta |
| 9 | [Email / SMTP](#9-email--smtp) | ⬜ Kutilmoqda | 🟡 O'rta |
| 10 | [Cloudflare](#10-cloudflare-cdn--waf) | ⬜ Kutilmoqda | 🟡 O'rta |
| 11 | [Server / Infrastructure](#11-server--infrastructure) | ⬜ Kutilmoqda | 🔴 Kritik |
| 12 | [Kontent ma'lumotlari](#12-klinika-kontent-malumotlari) | ⬜ Kutilmoqda | 🔴 Kritik |
| 13 | [Monitoring](#13-monitoring-va-error-tracking) | ⬜ Kutilmoqda | 🟡 O'rta |
| 14 | [Yuridik hujjatlar](#14-yuridik-hujjatlar) | ⬜ Kutilmoqda | 🟠 Yuqori |

---

## 1. Klinika Umumiy Ma'lumotlari

> **Qachon kerak:** Sprint 1 (1-2 hafta)  
> **Kim beradi:** Klinika rahbariyati

| # | Ma'lumot | Namuna | Holati |
|---|---------|--------|--------|
| 1.1 | Klinika to'liq nomi | "Med-Core Professional Klinikasi" | ⬜ |
| 1.2 | Klinika qisqa nomi (sayt uchun) | "Med-Core" | ⬜ |
| 1.3 | Klinika yuridik nomi (rasmiy) | "Med-Core Professional" MCHJ | ⬜ |
| 1.4 | Bosh klinika manzili | Toshkent sh., Amir Temur ko'chasi, 100 | ⬜ |
| 1.5 | Filiallar manzillari (agar bo'lsa) | Filial #2: ..., Filial #3: ... | ⬜ |
| 1.6 | Ish vaqti (bosh klinika) | Dushanba-Shanba, 08:00-20:00 | ⬜ |
| 1.7 | Ish vaqti (filiallar) | Har biri alohida | ⬜ |
| 1.8 | Asosiy telefon raqami | +998 71 123-45-67 | ⬜ |
| 1.9 | Qo'shimcha telefon raqamlari | Call center, filiallar | ⬜ |
| 1.10 | Email manzili (rasmiy) | info@klinika.uz | ⬜ |
| 1.11 | Ijtimoiy tarmoqlar | Instagram, Telegram kanal, Facebook | ⬜ |
| 1.12 | INN/STIR (soliq raqami) | Yuridik hujjatlar uchun | ⬜ |
| 1.13 | Bank rekvizitlari | Hisob raqam, MFO, bank nomi | ⬜ |
| 1.14 | Litsenziya raqami | Tibbiy faoliyat litsenziyasi | ⬜ |

---

## 2. Brending va Dizayn Materiallari

> **Qachon kerak:** Sprint 1 (1-2 hafta)  
> **Kim beradi:** Marketing bo'limi / Dizayner

| # | Material | Format/Talab | Holati |
|---|---------|--------------|--------|
| 2.1 | **Logo** (asosiy) | SVG + PNG (shaffof fon), min 1024×1024 px | ⬜ |
| 2.2 | **Logo** (oq rangli variant) | SVG + PNG (qorong'i fon uchun) | ⬜ |
| 2.3 | **Favicon** | ICO/PNG 32×32, 192×192, 512×512 | ⬜ |
| 2.4 | **Brend ranglari** | HEX/RGB kodlari (asosiy, ikkilamchi) | ⬜ |
| 2.5 | **Slogan** | "Sog'lom Hayot — Sizning Huquqingiz" yoki boshqa | ⬜ |
| 2.6 | **Hero video** | 15 sekund, loop, WebM+MP4, 1920×1080 min | ⬜ |
| 2.7 | **Klinika foto** (tashqi ko'rinish) | JPEG/PNG, high-res, kamida 5 ta | ⬜ |
| 2.8 | **Klinika foto** (ichki xonalar) | Qabul, kutish zali, kabinetlar | ⬜ |
| 2.9 | **360° panorama rasmlar** | Virtual tur uchun — har bir xona | ⬜ |
| 2.10 | **Open Graph rasmi** | 1200×630 px (Telegram/Facebook share) | ⬜ |

---

## 3. Domen va Hosting

> **Qachon kerak:** Sprint 1 (1-2 hafta)  
> **Kim beradi:** Klinika IT bo'limi / Buyurtmachi

| # | Ma'lumot | Izoh | Holati |
|---|---------|------|--------|
| 3.1 | **Domen nomi** | Masalan: `klinika.uz`, `med-core.uz` | ⬜ |
| 3.2 | **Domen registrator** | Domen qayerda ro'yxatdan o'tgan | ⬜ |
| 3.3 | **DNS boshqaruv kirish** | Domen DNS sozlamalarini o'zgartirish uchun | ⬜ |
| 3.4 | **SSL sertifikat** | Let's Encrypt (bepul) yoki maxsus sertifikat | ⬜ |
| 3.5 | **Subdomenlar rejasi** | `api.klinika.uz`, `admin.klinika.uz`, `cdn.klinika.uz` | ⬜ |

> ⚠️ **Muhim:** Agar domen hali sotib olinmagan bo'lsa, buyurtmachi tezroq ro'yxatdan o'tkazishi kerak. `.uz` domenlari UZINFOCOM orqali ro'yxatdan o'tkaziladi.

---

## 4. SMS Provayder Credentials

> **Qachon kerak:** Sprint 2 (3-4 hafta) — OTP tizimi uchun  
> **Kim beradi:** Buyurtmachi (shartnoma tuzib)  
> **Ishlatiladi:** OTP kod yuborish, eslatmalar, bildirishnomalar

### 4.1. Eskiz.uz (Tavsiya etiladigan)

| # | Ma'lumot | Izoh | Holati |
|---|---------|------|--------|
| 4.1.1 | **Eskiz.uz akkaunt** | https://eskiz.uz da ro'yxatdan o'tish | ⬜ |
| 4.1.2 | **API Login (email)** | Eskiz akkauntdagi email | ⬜ |
| 4.1.3 | **API Password** | Eskiz API paroli | ⬜ |
| 4.1.4 | **Sender Name (From)** | SMS yuboruvchi nomi (masalan: "MedCore") | ⬜ |
| 4.1.5 | **API Token** | `POST https://notify.eskiz.uz/api/auth/login` dan olinadi | ⬜ |
| 4.1.6 | **Shartnoma/balans** | SMS uchun balans to'ldirish (oyiga ~2000 SMS) | ⬜ |

### 4.2. Playmobile.uz (Muqobil)

| # | Ma'lumot | Izoh | Holati |
|---|---------|------|--------|
| 4.2.1 | **Playmobile akkaunt** | https://playmobile.uz da ro'yxatdan o'tish | ⬜ |
| 4.2.2 | **API Login** | API foydalanuvchi nomi | ⬜ |
| 4.2.3 | **API Password** | API paroli | ⬜ |
| 4.2.4 | **Sender Name** | SMS yuboruvchi nomi | ⬜ |

> 💡 **Eslatma:** Bitta SMS provayderning o'zi yetarli. **Eskiz.uz** tavsiya etiladi. Shartnoma tuzish 1-3 ish kuni olishi mumkin. Narx: ~50 so'm/sms.

---

## 5. To'lov Tizimlari (Click & Payme)

> **Qachon kerak:** Sprint 7 (13-14 hafta)  
> **Kim beradi:** Buyurtmachi (klinika yuridik shaxs sifatida shartnoma tuzib)  
> **Ishlatiladi:** Onlayn to'lov qabul qilish, avans to'lovlari, refund

### 5.1. Click.uz

| # | Ma'lumot | Izoh | Holati |
|---|---------|------|--------|
| 5.1.1 | **Merchant akkaunt** | https://merchant.click.uz da ro'yxatdan o'tish | ⬜ |
| 5.1.2 | **Service ID** | Click merchant paneldan olinadi | ⬜ |
| 5.1.3 | **Merchant ID** | Click merchant identifikator | ⬜ |
| 5.1.4 | **Secret Key** | API kaliti (callback tekshirish uchun) | ⬜ |
| 5.1.5 | **Merchant User ID** | API uchun foydalanuvchi | ⬜ |
| 5.1.6 | **Return URL** | `https://klinika.uz/booking/success` | ⬜ |
| 5.1.7 | **Callback URL** | `https://api.klinika.uz/api/v1/webhooks/click` | ⬜ |
| 5.1.8 | **Test muhit** | Click sandbox credentials (test uchun) | ⬜ |

> 📋 **Click uchun zarur hujjatlar:**
> - Yuridik shaxsning guvohnomasi
> - INN (STIR)
> - Tashkilot rahbarining pasporti nusxasi
> - Bank rekvizitlari
> - Shartnomaga imzo

### 5.2. Payme (Paycom)

| # | Ma'lumot | Izoh | Holati |
|---|---------|------|--------|
| 5.2.1 | **Merchant akkaunt** | https://merchant.paycom.uz da ro'yxatdan o'tish | ⬜ |
| 5.2.2 | **Merchant ID** | Payme merchant identifikator | ⬜ |
| 5.2.3 | **Secret Key (test)** | Test muhit uchun API kaliti | ⬜ |
| 5.2.4 | **Secret Key (production)** | Production API kaliti | ⬜ |
| 5.2.5 | **Callback URL** | `https://api.klinika.uz/api/v1/webhooks/payme` | ⬜ |
| 5.2.6 | **Account field nomi** | `booking_id` (buyurtma identifikatori) | ⬜ |
| 5.2.7 | **IP Whitelist** | Payme serverlar IP manzillari | ⬜ |
| 5.2.8 | **Checkout Key** | Payme checkout sahifasi uchun | ⬜ |
| 5.2.9 | **Test muhit** | Payme sandbox credentials | ⬜ |

> 📋 **Payme uchun zarur hujjatlar:**
> - Yuridik shaxsning guvohnomasi
> - STIR (INN)
> - Rahbar pasporti nusxasi  
> - Tashkilot tamg'asi (pechat)
> - Bank rekvizitlari
> - Shartnomaga imzo va tamg'a

> ⚠️ **Muhim:** Click va Payme shartnoma tuzish jarayoni **5-15 ish kuni** olishi mumkin. Shuning uchun **Sprint 5-6 da** allaqachon hujjatlar topshirilishi kerak!

---

## 6. Telegram Bot

> **Qachon kerak:** Sprint 2 (3-4 hafta)  
> **Kim beradi:** Buyurtmachi yoki biz yaratib beramiz  
> **Ishlatiladi:** OTP yuborish, eslatmalar, dori qabul eslatmasi, bemor bilan aloqa

| # | Ma'lumot | Izoh | Holati |
|---|---------|------|--------|
| 6.1 | **Bot nomi** | Masalan: `@MedCoreProfBot` | ⬜ |
| 6.2 | **Bot Token** | @BotFather dan olinadi | ⬜ |
| 6.3 | **Bot description** | Bot tavsifi (o'zbekcha) | ⬜ |
| 6.4 | **Bot avatar** | Bot profil rasmi (640×640 px) | ⬜ |
| 6.5 | **Webhook URL** | `https://api.klinika.uz/api/v1/webhooks/telegram` | ⬜ |
| 6.6 | **Telegram kanal** | Klinika yangiliklari kanali (ixtiyoriy) | ⬜ |
| 6.7 | **Telegram guruh** | Support guruh (ixtiyoriy) | ⬜ |

> 💡 **Yechim:** Biz @BotFather orqali bot yaratib, tokenni buyurtmachiga berishimiz mumkin. Yoki buyurtmachi o'zi yaratadi.

---

## 7. AI / OpenAI API

> **Qachon kerak:** Sprint 5 (9-10 hafta)  
> **Kim beradi:** Buyurtmachi (to'lov kartasi bilan)  
> **Ishlatiladi:** AI simptom diagnostika, tibbiy yo'naltiruvchi chatbot, tahlil sharhi

| # | Ma'lumot | Izoh | Holati |
|---|---------|------|--------|
| 7.1 | **OpenAI akkaunt** | https://platform.openai.com da ro'yxatdan o'tish | ⬜ |
| 7.2 | **API Key** | `sk-...` formatdagi API kaliti | ⬜ |
| 7.3 | **Organization ID** | (ixtiyoriy) tashkilot ID si | ⬜ |
| 7.4 | **To'lov usuli** | Karta ulash (oylik ~$30-50 taxminiy) | ⬜ |
| 7.5 | **Model tanlash** | GPT-4o (tavsiya) yoki GPT-4o-mini (arzonroq) | ⬜ |
| 7.6 | **Embedding model** | `text-embedding-ada-002` (vektor qidiruv uchun) | ⬜ |
| 7.7 | **Usage limits** | Oylik limitni belgilash ($50 masalan) | ⬜ |

> 💡 **Muqobil:** O'zbekistonda OpenAI bloklangan bo'lishi mumkin. Bu holda:
> - **OpenRouter** (openrouter.ai) — proxy API sifatida ishlatiladi
> - **Llama-3** (self-hosted) — bepul, lekin kuchli server talab qiladi
> - Buyurtmachi qaysi yondashuvni tanlashini aniqlashtirishi kerak

---

## 8. Google Xarita API

> **Qachon kerak:** Sprint 1 (1-2 hafta)  
> **Kim beradi:** Buyurtmachi (Google Cloud akkaunt kerak)  
> **Ishlatiladi:** Klinika joylashuvini ko'rsatish, yo'nalish hisoblash, ETA

| # | Ma'lumot | Izoh | Holati |
|---|---------|------|--------|
| 8.1 | **Google Cloud akkaunt** | https://console.cloud.google.com | ⬜ |
| 8.2 | **Maps JavaScript API Key** | Xarita embedding uchun | ⬜ |
| 8.3 | **Directions API** | Yo'nalish hisoblash uchun (ixtiyoriy) | ⬜ |
| 8.4 | **Geocoding API** | Manzilni koordinatalarga aylantirish | ⬜ |
| 8.5 | **API kalitni cheklash** | Faqat sayt domenidan ishlatish uchun | ⬜ |
| 8.6 | **Klinika koordinatalari** | Har bir filial uchun lat/lng | ⬜ |

> 💡 **Muqobil bepul yechim:** **Yandex Maps API** yoki **Leaflet + OpenStreetMap** (bepul).  
> Buyurtmachi qaysi xarita xizmatini xohlashini aniqlashtirishi kerak.

---

## 9. Email / SMTP

> **Qachon kerak:** Sprint 6 (11-12 hafta)  
> **Kim beradi:** Buyurtmachi  
> **Ishlatiladi:** Email bildirishnomalar, PDF ticket yuborish (ixtiyoriy kanal)

| # | Ma'lumot | Izoh | Holati |
|---|---------|------|--------|
| 9.1 | **SMTP server** | Gmail, Yandex yoki maxsus mail server | ⬜ |
| 9.2 | **SMTP host** | `smtp.gmail.com` yoki boshqa | ⬜ |
| 9.3 | **SMTP port** | 587 (TLS) yoki 465 (SSL) | ⬜ |
| 9.4 | **Email login** | noreply@klinika.uz | ⬜ |
| 9.5 | **Email password / App password** | SMTP autentifikatsiya uchun | ⬜ |
| 9.6 | **Sender name** | "Med-Core Professional" | ⬜ |

> 💡 **Eslatma:** Email ixtiyoriy kanal. Asosiy kanal — SMS + Telegram. Lekin PDF retseptlar va batafsil ma'lumotlar email orqali ham yuborilishi mumkin.

---

## 10. Cloudflare (CDN & WAF)

> **Qachon kerak:** Sprint 1-2 (1-4 hafta)  
> **Kim beradi:** Biz yaratib beramiz yoki buyurtmachi  
> **Ishlatiladi:** CDN, DDoS himoya, SSL, WAF

| # | Ma'lumot | Izoh | Holati |
|---|---------|------|--------|
| 10.1 | **Cloudflare akkaunt** | https://dash.cloudflare.com | ⬜ |
| 10.2 | **Tarif rejasi** | Free (bepul) yoki Pro ($20/oy - tavsiya) | ⬜ |
| 10.3 | **Zona (site)** | Domen Cloudflare ga ulangan bo'lishi kerak | ⬜ |
| 10.4 | **API Token** | Cloudflare API token (DNS boshqarish uchun) | ⬜ |
| 10.5 | **Origin CA sertifikat** | Server va Cloudflare orasidagi SSL | ⬜ |

---

## 11. Server / Infrastructure

> **Qachon kerak:** Sprint 1 (1-hafta!)  
> **Kim beradi:** Buyurtmachi (server sotib oladi yoki biz yordamlashamiz)  
> **Ishlatiladi:** Backend, Database, Redis, MinIO — barchasi shu serverda

### 11.1. Production Server (Minimal Talablar)

| # | Parametr | Minimal | Tavsiya | Holati |
|---|---------|---------|---------|--------|
| 11.1.1 | **vCPU** | 2 core | 4 core | ⬜ |
| 11.1.2 | **RAM** | 4 GB | 8 GB | ⬜ |
| 11.1.3 | **SSD Disk** | 80 GB | 160 GB | ⬜ |
| 11.1.4 | **OS** | Ubuntu 22.04 LTS | Ubuntu 22.04 LTS | ⬜ |
| 11.1.5 | **IP** | Statik IP manzili | Statik IP | ⬜ |
| 11.1.6 | **Bandwidth** | 1 Gbps | 1 Gbps | ⬜ |

### 11.2. Server Provayder Tanlov

| Provayder | Narx (taxminiy) | Izoh |
|-----------|-----------------|------|
| **Ahost.uz** | ~200-400K so'm/oy | O'zbekiston lokal |
| **InfoCOM** | ~300-500K so'm/oy | O'zbekiston lokal |
| **Hetzner** | $10-30/oy | Yevropa, arzon |
| **DigitalOcean** | $20-50/oy | Ishonchli |
| **AWS Lightsail** | $20-40/oy | Keng imkoniyatlar |

### 11.3. Server Kirish Ma'lumotlari

| # | Ma'lumot | Izoh | Holati |
|---|---------|------|--------|
| 11.3.1 | **SSH host** | Server IP manzili | ⬜ |
| 11.3.2 | **SSH port** | Odatda 22 | ⬜ |
| 11.3.3 | **SSH user** | root yoki sudo user | ⬜ |
| 11.3.4 | **SSH kalit / parol** | SSH key pair (tavsiya) | ⬜ |
| 11.3.5 | **Server provider panel** | Boshqaruv paneli kirish | ⬜ |

---

## 12. Klinika Kontent Ma'lumotlari

> **Qachon kerak:** Sprint 1-3 (1-6 hafta) — bosqichma-bosqich  
> **Kim beradi:** Klinika marketing bo'limi, menejment, shifokorlar  
> **Ishlatiladi:** Sayt kontenti, AI bilimlar bazasi, CRM dastlabki ma'lumotlar

### 12.1. Yo'nalishlar (37 ta)

Har bir yo'nalish uchun:

| # | Ma'lumot | Holati |
|---|---------|--------|
| 12.1.1 | Yo'nalish nomi (o'zbekcha va ruscha) | ⬜ |
| 12.1.2 | Qisqacha tavsifi (150-200 so'z) | ⬜ |
| 12.1.3 | Ko'rsatiladigan xizmatlar ro'yxati | ⬜ |
| 12.1.4 | Har bir xizmat narxi (so'mda) | ⬜ |
| 12.1.5 | FAQ (ko'p so'raladigan savollar) — 3-5 ta | ⬜ |

### 12.2. Shifokorlar

Har bir shifokor uchun:

| # | Ma'lumot | Holati |
|---|---------|--------|
| 12.2.1 | To'liq ismi (FIO) | ⬜ |
| 12.2.2 | Profil rasmi (yuzga qaragan, professional) | ⬜ |
| 12.2.3 | Yo'nalishi (masalan: Kardiologiya) | ⬜ |
| 12.2.4 | Mutaxassisligi (masalan: "Kardiolog, Oliy toifa") | ⬜ |
| 12.2.5 | Ish tajribasi (yillar soni) | ⬜ |
| 12.2.6 | Qisqacha biografiya (50-100 so'z) | ⬜ |
| 12.2.7 | Ish grafigi (qaysi kunlar, soatlar) | ⬜ |
| 12.2.8 | Qabul narxi (so'mda) | ⬜ |
| 12.2.9 | Slot davomiyligi (masalan: 30 daqiqa) | ⬜ |
| 12.2.10 | Onlayn konsultatsiya qilishi (ha/yo'q) | ⬜ |
| 12.2.11 | Telefon raqami (ichki tizim uchun) | ⬜ |

### 12.3. Tibbiy Protokollar (AI uchun)

| # | Ma'lumot | Izoh | Holati |
|---|---------|------|--------|
| 12.3.1 | Klinik protokollar | PDF/Word — SSV standartlari | ⬜ |
| 12.3.2 | MKB-10 klassifikatsiya fayli | O'zbekcha tarjima bilan | ⬜ |
| 12.3.3 | Dorilar ro'yxati | Klinikada mavjud dorilar | ⬜ |
| 12.3.4 | Simptom-yo'nalish mos jadvali | Qaysi simptomda qaysi shifokor | ⬜ |

### 12.4. Ombor Dastlabki Ma'lumotlari

| # | Ma'lumot | Holati |
|---|---------|--------|
| 12.4.1 | Barcha dori-darmonlar ro'yxati | ⬜ |
| 12.4.2 | Barcha sarflanadigan materiallar ro'yxati | ⬜ |
| 12.4.3 | Har bir mahsulot uchun minimal miqdor | ⬜ |
| 12.4.4 | Har bir mahsulot uchun joriy qoldiq | ⬜ |
| 12.4.5 | Yetkazib beruvchilar ro'yxati | ⬜ |
| 12.4.6 | Muolaja ↔ Material mos jadval | ⬜ |

### 12.5. Laboratoriya

| # | Ma'lumot | Holati |
|---|---------|--------|
| 12.5.1 | Tahlil turlari ro'yxati (kodlar bilan) | ⬜ |
| 12.5.2 | Har bir tahlil narxi | ⬜ |
| 12.5.3 | Referens qiymatlar (norma diapazonlari) | ⬜ |
| 12.5.4 | Tahlil bajarilish muddati | ⬜ |

---

## 13. Monitoring va Error Tracking

> **Qachon kerak:** Sprint 8 (15-17 hafta)  
> **Kim beradi:** Biz sozlaymiz, lekin akkauntlar kerak

| # | Xizmat | Ma'lumot | Holati |
|---|--------|---------|--------|
| 13.1 | **Sentry** | Akkaunt + DSN | ⬜ |
| 13.2 | **Grafana Cloud** | Akkaunt (bepul zarur) | ⬜ |
| 13.3 | **UptimeRobot** | Monitoring akkaunt (bepul) | ⬜ |

> 💡 Bularni biz yaratib sozlashimiz mumkin. Buyurtmachi email berishining o'zi yetarli.

---

## 14. Yuridik Hujjatlar

> **Qachon kerak:** Sprint 7-8 (13-17 hafta) — launch oldidan  
> **Kim beradi:** Klinika yurist / rahbariyat

| # | Hujjat | Nima uchun kerak | Holati |
|---|--------|-----------------|--------|
| 14.1 | **Foydalanish shartlari** | Saytda joylashtirish uchun | ⬜ |
| 14.2 | **Maxfiylik siyosati** | Shaxsiy ma'lumotlarni qayta ishlash | ⬜ |
| 14.3 | **Tibbiy ma'lumotlar roziligi** | EHR uchun bemor roziligi matni | ⬜ |
| 14.4 | **Cookie siyosati** | GDPR uchun | ⬜ |
| 14.5 | **To'lov qaytarish siyosati** | Click/Payme uchun zarur | ⬜ |
| 14.6 | **AI javobgarlik bayoni** | AI tashxis qo'ymasligini bildirish | ⬜ |

---

## 📅 Sprint Bo'yicha Kerak Muddatlari

```
Sprint    Muddat          Qaysi ma'lumotlar kerak
───────────────────────────────────────────────────────────────
Sprint 1  Hafta 1-2       ✅ Klinika info, brending, domen, server
Sprint 2  Hafta 3-4       ✅ SMS provayder, Telegram bot token
Sprint 3  Hafta 5-6       ✅ Shifokorlar ma'lumotlari, narxlar
Sprint 5  Hafta 9-10      ✅ OpenAI API key, tibbiy protokollar
Sprint 6  Hafta 11-12     ✅ Email SMTP (ixtiyoriy)
Sprint 7  Hafta 13-14     ✅ Click + Payme credentials (!!!)
Sprint 8  Hafta 15-17     ✅ Yuridik hujjatlar, monitoring
```

---

## ⚠️ Eng Muhim va Vaqt Talab Qiladigan Elementlar

> [!CAUTION]
> Quyidagi elementlar **shartnoma tuzishni** talab qiladi va **5-15 ish kuni** olishi mumkin.  
> Shuning uchun IMKON QADAR TEZROQ boshlash kerak!

| # | Element | Shartnoma vaqti | Tavsiya: Qachon boshlash |
|---|---------|-----------------|-------------------------|
| 1 | **Click.uz merchant** | 5-10 ish kuni | Sprint 4 da (7-hafta) |
| 2 | **Payme merchant** | 7-15 ish kuni | Sprint 4 da (7-hafta) |
| 3 | **SMS provayder (Eskiz)** | 1-3 ish kuni | Sprint 1 da (1-hafta) |
| 4 | **Domen (.uz)** | 1-5 ish kuni | Hoziroq! |
| 5 | **Server sotib olish** | 1 kun (cloud) | Hoziroq! |

---

## 📝 Buyurtmachi Uchun Qisqa Xulosa

Buyurtmachi **minimal ravishda** quyidagilarni taqdim etishi kerak:

### 🔴 Darhol (1-haftada):
1. Klinika nomi, manzili, telefoni, ish vaqti
2. Logo (SVG/PNG), brend ranglar
3. Domen nomi
4. Server (yoki server uchun byudjet)

### 🟠 2-4 hafta ichida:
5. SMS provayder akkaunt (Eskiz.uz)
6. Telegram bot token
7. Barcha shifokorlar ro'yxati + foto + grafik + narx
8. 37 ta yo'nalish uchun xizmatlar va narxlar

### 🟡 5-10 hafta ichida:
9. OpenAI API kaliti (yoki muqobil AI xizmat)
10. Tibbiy protokollar, MKB-10, dorilar ro'yxati
11. Laboratoriya tahlillari va narxlari
12. Ombor dastlabki ma'lumotlari
13. Google Maps API key

### 🟢 11-15 hafta ichida:
14. Click merchant credentials
15. Payme merchant credentials
16. Yuridik hujjatlar (maxfiylik siyosati va boshqalar)
17. Email SMTP sozlamalari
18. 360° panorama rasmlar (virtual tur)

---

*© 2026 Med-Core Professional. Barcha huquqlar himoyalangan.*
