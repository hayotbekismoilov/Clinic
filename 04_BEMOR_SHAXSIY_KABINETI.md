# 04. Bemor Shaxsiy Kabinet (Patient Portal)

> **Modul:** Patient Personal Cabinet  
> **Texnologiya:** Next.js (SSR), JWT Auth, S3/MinIO (fayl saqlash)  
> **Mas'ul:** Frontend Developer + Backend Developer  

---

## 1.1. Autentifikatsiya (Kirish Tizimi)

### 1.1.1. Kirish Oqimi

```
KIRISH USULLARI:

1️⃣ TELEFON RAQAM + OTP (Asosiy):
   ┌──────────────────────────┐
   │  📱 Kirish               │
   │                          │
   │  Telefon raqamingiz:     │
   │  +998 [__ ___ __ __]     │
   │                          │
   │  [Kod Yuborish]          │
   └────────────┬─────────────┘
                │
                ▼
   ┌──────────────────────────┐
   │  🔑 OTP Tasdiqlash       │
   │                          │
   │  SMS kod:                │
   │  [_ _ _ _ _ _]           │
   │                          │
   │  59 sek — Qayta yuborish │
   │                          │
   │  [Kirish]                │
   └──────────────────────────┘

2️⃣ TELEGRAM ONE-TAP (Tez kirish):
   ┌──────────────────────────┐
   │  💬 Telegram orqali      │
   │                          │
   │  [@MedCoreBot] ga        │
   │  /login buyrug'ini       │
   │  yuboring                │
   │                          │
   │  [Telegram da Ochish]    │
   └──────────────────────────┘

3️⃣ BIOMETRIKA (PWA uchun, kelajakda):
   Fingerprint / Face ID → Tez kirish
```

### 1.1.2. Session Boshqaruvi

```
AUTENTIFIKATSIYA TEXNIK TAFSILOTLARI:

Token turi:     JWT (JSON Web Token)
Access Token:   15 daqiqa (qisqa, xavfsiz)
Refresh Token:  30 kun (faqat httpOnly cookie da)
Saqlash:        Redis (server-side session + token blacklist)

Token Payload:
{
  "sub": "PAT-00001234",          // Patient ID
  "phone": "+998901234567",
  "role": "PATIENT",
  "permissions": ["read:own_records", "create:bookings"],
  "iat": 1741513200,
  "exp": 1741514100
}

Har bir so'rovda:
  Authorization: Bearer <access_token>
  
Token yangilash:
  POST /api/v1/auth/refresh
  Cookie: refresh_token=<token>
```

---

## 1.2. Dashboard (Bosh Sahifa)

### 1.2.1. Shaxsiy Kabinet Wireframe

```
┌──────────────────────────────────────────────────────────────┐
│  🏥 Med-Core                    👤 Ahmadov Sherzod  [🔔 3]  │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  Salom, Sherzod! 👋                                         │
│                                                              │
│  ┌────────────────────────────────────────────────────┐      │
│  │  📅 YAQINLASHAYOTGAN NAVBATLAR                     │      │
│  │  ─────────────────────────────                     │      │
│  │                                                    │      │
│  │  ┌──────────────────────────────────────┐          │      │
│  │  │  ❤️ Kardiolog — Dr. Aliyev           │          │      │
│  │  │  📅 15-Mart, 2026  ⏰ 10:00          │          │      │
│  │  │  📍 Bosh klinika, 2-qavat, 204-xona │          │      │
│  │  │  Status: ✅ Tasdiqlangan              │          │      │
│  │  │                                      │          │      │
│  │  │  [📅 Ko'chirish] [❌ Bekor Qilish]   │          │      │
│  │  └──────────────────────────────────────┘          │      │
│  │                                                    │      │
│  │  [+ Yangi Navbat Olish]                            │      │
│  └────────────────────────────────────────────────────┘      │
│                                                              │
│  ┌───────────────┐ ┌───────────────┐ ┌───────────────┐      │
│  │  🔬 Tahlillar │ │  💊 Dorilar   │ │  📋 Tarix     │      │
│  │    3 ta yangi │ │   2 ta aktiv │ │   12 tashrif  │      │
│  │  [Ko'rish →]  │ │  [Ko'rish →] │ │  [Ko'rish →]  │      │
│  └───────────────┘ └───────────────┘ └───────────────┘      │
│                                                              │
│  ┌────────────────────────────────────────────────────┐      │
│  │  📊 SOG'LIQIM HOLATI                               │      │
│  │                                                    │      │
│  │  Qon bosimi (oxirgi 30 kun):                       │      │
│  │  140─┤          ╱╲                                 │      │
│  │  130─┤    ╱╲  ╱    ╲   ╱╲                          │      │
│  │  120─┤  ╱    ╲╱      ╲╱  ╲──── norma              │      │
│  │  110─┤╱                                            │      │
│  │  100─┤────────────────────────────                 │      │
│  │      └──┬──┬──┬──┬──┬──┬──┬──┬──                  │      │
│  │        5  10 15 20 25 1  5  10                     │      │
│  │       Fevral              Mart                     │      │
│  └────────────────────────────────────────────────────┘      │
│                                                              │
│  ┌───────┐ ┌───────┐ ┌───────┐ ┌───────┐                    │
│  │🏠Bosh  │ │📅Navbt│ │🔬Tahlil│ │👤Profil│                  │
│  └───────┘ └───────┘ └───────┘ └───────┘                    │
└──────────────────────────────────────────────────────────────┘
```

---

## 1.3. Tahlillar Bazasi (Lab Results)

### 1.3.1. Tahlil Natijalari Ko'rinishi

```
┌──────────────────────────────────────────────────────────────┐
│  🔬 TAHLILLAR                                    [📥 Barchasi]│
│  ─────────                                                    │
│                                                               │
│  Filter: [Barchasi ▼] [Oxirgi 30 kun ▼] [🔍 Qidirish]       │
│                                                               │
│  ┌────────────────────────────────────────────────────────┐   │
│  │  📄 Umumiy Qon Tahlili (CBC)                           │   │
│  │  📅 10-Mart, 2026                                      │   │
│  │  👨‍⚕️ Dr. Karimova N. (Terapevt)                       │   │
│  │  Status: 🟢 Tayyor                                     │   │
│  │                                                        │   │
│  │  ┌──────────────┬────────┬───────────┬────────┐        │   │
│  │  │ Ko'rsatkich   │ Natija │ Norma     │ Status │        │   │
│  │  ├──────────────┼────────┼───────────┼────────┤        │   │
│  │  │ Hemoglobin   │ 145    │ 130-160   │ ✅     │        │   │
│  │  │ Eritrotsitlar│ 4.8    │ 4.0-5.5   │ ✅     │        │   │
│  │  │ Leykotsitlar │ 12.5   │ 4.0-9.0   │ ⚠️ ↑   │        │   │
│  │  │ Trombotsitlar│ 250    │ 180-320   │ ✅     │        │   │
│  │  │ SOE          │ 18     │ 2-15      │ ⚠️ ↑   │        │   │
│  │  └──────────────┴────────┴───────────┴────────┘        │   │
│  │                                                        │   │
│  │  ⚠️ 2 ta ko'rsatkich normadan yuqori                   │   │
│  │  💡 AI sharh: "Leykotsitlar va SOE biroz yuqori.       │   │
│  │     Bu yallig'lanish mavjudligini ko'rsatishi mumkin.  │   │
│  │     Shifokoringiz bilan maslahatlashing."              │   │
│  │                                                        │   │
│  │  [📥 PDF Yuklab Olish] [📊 Grafik Ko'rish]             │   │
│  │  [📋 Avvalgisi bilan Solishtirish]                     │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                               │
│  ┌────────────────────────────────────────────────────────┐   │
│  │  📄 Bioximik Qon Tahlili                               │   │
│  │  📅 8-Mart, 2026                                       │   │
│  │  Status: 🟢 Tayyor                                     │   │
│  │  [Batafsil Ko'rish →]                                  │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

### 1.3.2. Tahlillarni Solishtirish (Compare View)

```
SOLISHTIRISH KO'RINISHI:

┌──────────────────────────────────────────────────────────┐
│  📊 Tahlillarni Solishtirish                              │
│                                                           │
│  Ko'rsatkich: Hemoglobin (g/L)                           │
│  Norma diapazoni: 130-160 (yashil zona)                  │
│                                                           │
│  170─┤                                                    │
│  160─┤─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─  (yuqori norma)   │
│  150─┤         ○                                         │
│  145─┤                   ○        ○     (oxirgi natija)  │
│  140─┤    ○                                              │
│  130─┤─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─  (past norma)     │
│  120─┤                                                    │
│      └──┬────────┬────────┬────────┬──                   │
│       Jan-26   Apr-26   Jul-26  Oct-26                   │
│                                                           │
│  Trend: ✅ Barqaror (norma doirasida)                    │
└──────────────────────────────────────────────────────────┘
```

### 1.3.3. AI Sharh (Tahlil Tarjimoni)

```
AI TAHLIL SHARH ALGORITMI:

1. Laboratoriya tahlilni tizimga kiritadi
   │
2. AI modul avtomatik ravishda:
   ├── Normadan og'igan ko'rsatkichlarni aniqlaydi
   ├── Avvalgi tahlillar bilan solishtiradi (trend)
   ├── Oddiy tilda tushuntirish tayyorlaydi
   └── Shoshilinchlik darajasini belgilaydi
   │
3. Natija bemorga ko'rsatiladi:
   │
   ├── 🟢 "Barcha ko'rsatkichlar normada. Sog'ligingiz yaxshi!"
   ├── 🟡 "2 ta ko'rsatkich biroz normadan yuqori. 
   │       Bu tabiiy xavfsiz sabablari bo'lishi mumkin,
   │       lekin shifokor bilan gaplashing."
   └── 🔴 "Muhim ko'rsatkich normadan ancha farq qiladi. 
          Imkon qadar tezroq shifokoringizga murojaat qiling."

⚠️ MUHIM: AI hech qachon tashxis qo'ymaydi.
   Faqat tushunarli tilda sharh beradi.
```

---

## 1.4. Elektron Retseptlar (E-Prescriptions)

### 1.4.1. Retsept Ko'rinishi

```
┌──────────────────────────────────────────────────────────────┐
│  💊 ELEKTRON RETSEPT                                         │
│  ──────────────────                                          │
│                                                               │
│  ┌────────────────────────────────────────────────────────┐   │
│  │  📋 Retsept #RX-2026-00234                             │   │
│  │  📅 10-Mart, 2026                                      │   │
│  │  👨‍⚕️ Dr. Aliyev Xasan (Kardiolog)                      │   │
│  │  Tashxis: I10 — Gipertoniya                            │   │
│  │                                                        │   │
│  │  ┌───┬──────────────┬────────────┬──────────────────┐  │   │
│  │  │ # │ Dori nomi     │ Dozasi     │ Qabul tartibi    │  │   │
│  │  ├───┼──────────────┼────────────┼──────────────────┤  │   │
│  │  │ 1 │ Amlodipine   │ 5 mg       │ Ertalab 1 ta     │  │   │
│  │  │ 2 │ Lisinopril   │ 10 mg      │ Ertalab 1 ta     │  │   │
│  │  │ 3 │ Aspirin      │ 75 mg      │ Kechqurun 1 ta   │  │   │
│  │  └───┴──────────────┴────────────┴──────────────────┘  │   │
│  │                                                        │   │
│  │  📌 Shifokor izohi:                                    │   │
│  │  "Tuzli ovqatni kamaytiring. 2 haftadan keyin          │   │
│  │   kontrolga keling."                                    │   │
│  │                                                        │   │
│  │  ┌─────────────┐                                       │   │
│  │  │ [QR CODE]   │  Dorixonada shu QR ni ko'rsating     │   │
│  │  │             │  va dorilarni oling.                   │   │
│  │  └─────────────┘                                       │   │
│  │                                                        │   │
│  │  Amal qilish muddati: 10-Mart → 10-Aprel, 2026       │   │
│  │                                                        │   │
│  │  [📥 PDF] [📤 Telegram] [🔔 Eslatma O'rnatish]        │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

### 1.4.2. Dori Qabul Eslatmasi

```
DORI QABUL ESLATMA TIZIMI:

Bemor "Eslatma O'rnatish" tugmasini bosadi:
│
├── Tizim retseptdagi vaqtlarni aniqlaydi:
│   ├── Amlodipine: Har kuni ertalab — 08:00
│   ├── Lisinopril: Har kuni ertalab — 08:00
│   └── Aspirin: Har kuni kechqurun — 20:00
│
├── Telegram bot orqali eslatma:
│   08:00 → "💊 Ertalabki dorilar vaqti:
│            1. Amlodipine 5mg
│            2. Lisinopril 10mg
│            [✅ Qabul qildim] [⏰ 30 daqiqadan keyin]"
│
│   20:00 → "💊 Kechki dori vaqti:
│            1. Aspirin 75mg
│            [✅ Qabul qildim]"
│
└── Statistika:
    "Bu haftada 14/14 (100%) dorilarni vaqtida qabul qildingiz! 🎉"
```

---

## 1.5. Qayta Yozilish (One-Click Re-Booking)

### 1.5.1. Oqim

```
QAYTA YOZILISH ALGORITMI:

1. Bemor "Qayta yozilish" tugmasini bosadi
   │
2. Tizim avtomatik ravishda:
   ├── Avvalgi shifokorni tanlaydi (Dr. Aliyev)
   ├── Avvalgi yo'nalishni tanlaydi (Kardiologiya)
   ├── Eng yaqin bo'sh slotni topadi
   ├── Bemor ma'lumotlarini avvalgisidan oladi (qayta kiritish shart emas)
   └── Avvalgi tahlillar va tashxis tarixini bemor kartasiga biriktiradi
   │
3. Bemorga faqat:
   ├── Sana/vaqtni tasdiqlash → 1 tugma
   └── To'lovni amalga oshirish → Click/Payme
   │
4. TAYYOR! (Maximum 30 sekundda bron yakunlanadi)
```

---

## 1.6. Bildirishnomalar Markazi

### 1.6.1. In-App Notifications

```
┌──────────────────────────────────────────────────┐
│  🔔 BILDIRISHNOMALAR                     [Barchasi]│
│  ──────────────────                               │
│                                                    │
│  🆕 Yangi (3)                                     │
│  ┌──────────────────────────────────────────┐     │
│  │  🔬 Tahlil natijangiz tayyor!            │     │
│  │  Umumiy Qon Tahlili — 10-Mart            │     │
│  │  🕐 2 soat oldin                         │     │
│  │  [Ko'rish →]                             │     │
│  ├──────────────────────────────────────────┤     │
│  │  📅 Eslatma: Ertaga qabulingiz bor       │     │
│  │  Dr. Aliyev — 10:00                      │     │
│  │  🕐 5 soat oldin                         │     │
│  │  [Tasdiqlash] [Ko'chirish]               │     │
│  ├──────────────────────────────────────────┤     │
│  │  💊 Dori qabul qilish vaqti!             │     │
│  │  Amlodipine 5mg — Ertalabki doza         │     │
│  │  🕐 Bugun 08:00                          │     │
│  │  [✅ Qildim] [⏰ Keyinroq]               │     │
│  └──────────────────────────────────────────┘     │
│                                                    │
│  📖 O'qilgan                                      │
│  ┌──────────────────────────────────────────┐     │
│  │  ✅ Navbatingiz tasdiqlandi               │     │
│  │  🕐 2 kun oldin                           │     │
│  └──────────────────────────────────────────┘     │
│                                                    │
└──────────────────────────────────────────────────┘
```

---

## 1.7. Profil Sozlamalari

### 1.7.1. Profil Sahifasi

```
PROFIL MA'LUMOTLARI:

┌──────────────────────────────────────────────────────────┐
│  👤 PROFIL SOZLAMALARI                                    │
│                                                           │
│  ┌─────────────────────────────────────────────┐          │
│  │  📷 [Rasm]                                   │          │
│  │                                              │          │
│  │  Ism:        Ahmadov Sherzod                 │          │
│  │  Telefon:    +998 90 123-45-67  [✏️ O'zgartir]│          │
│  │  Email:      sherzod@gmail.com (ixtiyoriy)   │          │
│  │  Tug'ilgan:  15.05.1990                      │          │
│  │  Jinsi:      Erkak                           │          │
│  │  Qon guruhi: A(II) Rh+                      │          │
│  │  Allergiya:  Penitsillinga allergik           │          │
│  │  Telegram:   @sherzod90 (ulangan ✅)         │          │
│  └─────────────────────────────────────────────┘          │
│                                                           │
│  ⚙️ SOZLAMALAR                                            │
│  ├── 🔔 Bildirishnomalar: [SMS ✅] [Telegram ✅] [Email ❌]│
│  ├── 🌐 Til: [O'zbek ▼]                                   │
│  ├── 🌙 Qorong'i rejim: [○ ● ]                            │
│  └── 🗑️ Hisobni o'chirish                                 │
│                                                           │
│  [💾 Saqlash]                                              │
└──────────────────────────────────────────────────────────┘
```

---

*Keyingi bo'lim: [05_CRM_TIBBIY_AXBOROT_TIZIMI.md](./05_CRM_TIBBIY_AXBOROT_TIZIMI.md)*
