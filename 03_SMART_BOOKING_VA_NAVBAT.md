# 03. Smart Booking va Onlayn Navbat Tizimi

> **Modul:** Smart Booking Engine  
> **Texnologiya:** NestJS, PostgreSQL, Redis, Socket.IO, BullMQ  
> **Mas'ul:** Backend Lead + Frontend Developer  

---

## 1.1. Umumiy Arxitektura

### 1.1.1. Booking Oqimi (End-to-End)

```
┌─────────────────────────────────────────────────────────────────┐
│                    SMART BOOKING OQIMI                           │
│                                                                  │
│  1️⃣ SHIFOKOR TANLASH                                            │
│  ┌──────┐                                                        │
│  │Bemor │──▶ Yo'nalishni tanlaydi (yoki AI tavsiya qiladi)      │
│  └──┬───┘                                                        │
│     │                                                            │
│  2️⃣ VAQT TANLASH                                                │
│     ▼                                                            │
│  ┌────────────────────────────────────┐                          │
│  │  Kalendar: Bo'sh slotlar real-vaqt │                          │
│  │  ┌─────┬─────┬─────┬─────┬─────┐  │                          │
│  │  │09:00│09:30│10:00│10:30│11:00│  │                          │
│  │  │ ✅  │ ❌  │ ✅  │ ✅  │ ❌  │  │                          │
│  │  └─────┴─────┴─────┴─────┴─────┘  │                          │
│  └────────────────┬───────────────────┘                          │
│                   │                                              │
│  3️⃣ MA'LUMOT KIRITISH                                           │
│                   ▼                                              │
│  ┌──────────────────────────────────┐                            │
│  │  Ism: ___________________       │                            │
│  │  Tel: +998 __ ___ __ __         │                            │
│  │  Tug'ilgan sana: __/__/____     │                            │
│  │  Izoh: ___________________      │                            │
│  └──────────────┬───────────────────┘                            │
│                 │                                                │
│  4️⃣ OTP TASDIQLASH                                              │
│                 ▼                                                │
│  ┌────────────────────────────────┐                              │
│  │  📱 SMS Kod: [_ _ _ _ _ _]    │                              │
│  │  Qayta yuborish: 59 sek       │                              │
│  └──────────────┬─────────────────┘                              │
│                 │                                                │
│  5️⃣ TO'LOV (Ixtiyoriy yoki Majburiy)                           │
│                 ▼                                                │
│  ┌────────────────────────────────┐                              │
│  │  💳 To'lov:                    │                              │
│  │  ○ Click  ○ Payme  ○ Naqd     │                              │
│  │  Summa: 80,000 so'm           │                              │
│  │  Zaklad (20%): 16,000 so'm    │                              │
│  └──────────────┬─────────────────┘                              │
│                 │                                                │
│  6️⃣ TASDIQLASH                                                  │
│                 ▼                                                │
│  ┌────────────────────────────────┐                              │
│  │  ✅ Navbatingiz tasdiqlandi!   │                              │
│  │                                │                              │
│  │  📋 Ma'lumotlar:               │                              │
│  │  Shifokor: Dr. Aliyev          │                              │
│  │  Sana: 15-mart, 2026           │                              │
│  │  Vaqt: 10:00                   │                              │
│  │  Manzil: Amir Temur, 100      │                              │
│  │  Kod: #MED-2026-0415          │                              │
│  │                                │                              │
│  │  [📥 Kalendarimga Qo'shish]    │                              │
│  │  [📤 Telegram da Saqlash]      │                              │
│  └────────────────────────────────┘                              │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 1.2. Vaqt Slotlari Boshqaruvi

### 1.2.1. Slot Tuzilishi

```
SHIFOKOR GRAFIGI MODELI:

Shifokor: Dr. Aliyev Xasan (Kardiolog)
Ish kunlari: Dushanba-Juma
Ish vaqti: 09:00 — 17:00
Slot davomiyligi: 30 daqiqa
Tushlik: 13:00 — 14:00

Bir kunlik grafik:
┌────────────────────────────────────────────────┐
│  09:00 │ ✅ Bo'sh                               │
│  09:30 │ 🔴 Band — Ahmadov Sherzod             │
│  10:00 │ ✅ Bo'sh                               │
│  10:30 │ 🔴 Band — Karimova Nilufar            │
│  11:00 │ ✅ Bo'sh                               │
│  11:30 │ 🟡 Kutilmoqda (OTP tasdiqlanmagan)    │
│  12:00 │ ✅ Bo'sh                               │
│  12:30 │ ✅ Bo'sh                               │
│  ────── TUSHLIK ──────                          │
│  14:00 │ 🔴 Band — Saidov Ali                  │
│  14:30 │ ✅ Bo'sh                               │
│  15:00 │ 🟠 Onlayn konsultatsiya               │
│  15:30 │ ✅ Bo'sh                               │
│  16:00 │ ✅ Bo'sh                               │
│  16:30 │ 🔴 Band — To'rayev Bobur             │
│  17:00 │ ─── Ish tugadi ───                     │
└────────────────────────────────────────────────┘

STATUS ranglar:
  ✅ Bo'sh (AVAILABLE)     — bron qilish mumkin
  🔴 Band (BOOKED)        — tasdiqlangan
  🟡 Kutilmoqda (PENDING) — OTP tasdiqlanmagan (15 daqiqa limit)
  🟠 Onlayn (ONLINE)      — masofaviy qabul
  ⬛ Bloklangan (BLOCKED)  — shifokor tomonidan yopilgan
```

### 1.2.2. Double-Booking Himoyasi

```
DOUBLE-BOOKING PREVENTION ALGORITMI:

1. Bemor slot tanlaydi
   │
2. Server slot holatini tekshiradi → PostgreSQL "SELECT FOR UPDATE" 
   │                                  (pessimistic locking)
   │
3. Agar AVAILABLE:
   │  ├── Status → PENDING
   │  ├── 15 daqiqalik timer boshlanadi
   │  ├── OTP yuboriladi
   │  └── Boshqa foydalanuvchilarga bu slot "band" ko'rinadi
   │
4. Agar PENDING (boshqa bemor tanlagan):
   │  └── "Bu vaqt hozirda band qilinmoqda, iltimos boshqa vaqtni tanlang"
   │
5. OTP tasdiqlansa:
   │  ├── Status → BOOKED
   │  └── CRM ga yoziladi
   │
6. 15 daqiqa o'tsa (OTP tasdiqlanmasa):
   │  ├── Status → AVAILABLE (qayta ochiladi)
   │  └── Boshqa bemorlar uchun bo'shatiladi
   │
7. WebSocket orqali barcha onlayn foydalanuvchilarga slot holati
   real-vaqtda yangilanadi
```

### 1.2.3. Real-Time Sync (WebSocket)

```javascript
// Socket.IO events:

// Server → Client: Slot holati o'zgarganda
socket.emit('slot:updated', {
  doctorId: 'doc_123',
  date: '2026-03-15',
  slotTime: '10:00',
  status: 'BOOKED',      // AVAILABLE | PENDING | BOOKED | BLOCKED
  updatedAt: '2026-03-09T10:30:00Z'
});

// Server → Client: Yangi kun ochilganda
socket.emit('schedule:changed', {
  doctorId: 'doc_123',
  date: '2026-03-20',
  availableSlots: ['09:00', '10:00', '10:30', '14:00', '15:30']
});

// Client → Server: Foydalanuvchi slotni ko'rayotganini bildirish
socket.emit('slot:viewing', {
  doctorId: 'doc_123',
  date: '2026-03-15',
  slotTime: '10:00'
});
```

---

## 1.3. OTP Tasdiqlash Tizimi

### 1.3.1. OTP Oqimi

```
OTP LIFECYCLE:

1. Bemor telefon raqami: +998 90 123-45-67
   │
2. Server 6-xonali kod generatsiya qiladi: 847291
   │
3. Redis'da saqlanadi:
   │  Key:   "otp:+998901234567"
   │  Value: "847291"
   │  TTL:   300 sekund (5 daqiqa)
   │
4. SMS Gateway orqali yuboriladi:
   │  "Med-Core: Sizning tasdiqlash kodingiz: 847291. 5 daqiqa ichida kiriting."
   │
5. Bemor kodni kiritadi
   │
6. Tekshirish:
   ├── ✅ To'g'ri → Bron tasdiqlandi
   ├── ❌ Noto'g'ri → "Kod noto'g'ri, qaytadan urinib ko'ring" (max 3 urinish)
   └── ⏰ Muddati o'tgan → "Kod eskirdi. Qayta yuborish" tugmasi

XAVFSIZLIK:
- Bitta raqamga kuniga max 10 ta OTP
- 3 ta noto'g'ri urinishdan keyin 30 daqiqa blok
- Brute-force himoyasi: rate limiting (5 req/min)
```

### 1.3.2. SMS Gateway Integratsiya

```
QUYIDAGILARDAN BIRI TANLANADI:

1. Eskiz.uz (O'zbekiston lokal provayder)
   - API: https://notify.eskiz.uz/api/
   - Narx: ~50 so'm/sms
   - O'zbek raqamlar uchun optimal

2. Playmobile.uz
   - API: REST
   - Narx: ~45 so'm/sms

3. Telegram Bot API (bepul alternativa)
   - Bemor telegram ID si bo'lsa, OTP telegram orqali yuboriladi
   - SMS ga nisbatan bepul va tezroq
```

---

## 1.4. Bildirishnomalar Tizimi (Notification Engine)

### 1.4.1. Bildirishnomalar Jadvali

```
NOTIFICATION TIMELINE:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📅 Bron qilingan payt (T=0):
   ├── ✅ SMS: "Navbatingiz tasdiqlandi. 
   │          Dr. Aliyev, 15-mart, 10:00. 
   │          Kod: #MED-2026-0415"
   ├── 📱 Telegram: Xuddi shu ma'lumot + inline tugmalar
   │                 [❌ Bekor qilish] [📅 Ko'chirish]
   └── 📧 Email (ixtiyoriy): Batafsil ma'lumot + PDF ticket

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📅 3 kun oldin (T-3 days):
   ├── 📱 Telegram: "Eslatma: 3 kundan so'ng shifokor qabulingiz.
   │                Dr. Aliyev, 15-mart, 10:00.
   │                Tayyorgarlik: Oxirgi tahlil natijalarini olib keling."
   └── 💡 Tayyorgarlik bo'yicha yo'riqnoma:
       └── "Qon tahlili uchun 12 soat ovqat yemaslik kerak"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📅 24 soat oldin (T-24h):
   ├── SMS: "Ertaga soat 10:00 da Dr. Aliyev qabuliga 
   │         kelishingizni kutamiz. 
   │         Tasdiqlang: 1-Kelaman, 2-Kela olmayman"
   ├── 📱 Telegram: Inline buttons:
   │                [✅ Kelaman] [❌ Kela olmayman] [📅 Ko'chirish]
   └── ❌ Agar "Kela olmayman" → slot avtomatik bo'shatiladi
       va navbatdagi bemor informatsiya oladi

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📅 2 soat oldin (T-2h):
   └── 📱 Push/Telegram: "2 soatdan so'ng qabul. 
                          Manzil: Amir Temur 100. 
                          [🗺️ Yo'nalish ko'rsatish]"

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🚨 FAVQULODDA (Emergency):
   Shifokor band bo'lib qolsa:
   ├── SMS/Telegram barcha yozilgan bemorlarga:
   │   "Kechirasiz, Dr. Aliyev bugun favqulodda sabab 
   │    qabul qila olmaydi. 
   │    Sizga boshqa shifokorni taklif qilamiz yoki 
   │    boshqa vaqtga ko'chiramiz."
   │   [👨‍⚕️ Boshqa shifokor] [📅 Boshqa kun] [💰 Pul qaytarish]
   └── Registratura avtomatik ogohlantiriladi

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📅 Qabuldan keyin (T+1h):
   └── 📱 Telegram: "Qabulingiz yakunlandi. 
                     Shifokor xizmatini baholang: ⭐⭐⭐⭐⭐
                     [1⭐] [2⭐] [3⭐] [4⭐] [5⭐]"
```

### 1.4.2. Texnik Implementatsiya (BullMQ)

```
BILDIRISHNOMA QUEUE ARXITEKTURASI:

┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  BOOKING     │     │  BULL MQ     │     │  WORKERS     │
│  SERVICE     │────▶│  QUEUE       │────▶│              │
│              │     │  (Redis)     │     │  ┌─── SMS    │
└──────────────┘     └──────────────┘     │  ├─── Tg Bot │
                                          │  ├─── Email  │
                                          │  └─── Push   │
                                          └──────────────┘

Job Types:
1. "notification:booking-confirmed"   → SMS + Telegram
2. "notification:reminder-3d"         → Telegram only
3. "notification:reminder-24h"        → SMS + Telegram
4. "notification:reminder-2h"         → Push + Telegram
5. "notification:emergency-cancel"    → SMS + Telegram + Push
6. "notification:post-visit-review"   → Telegram only
7. "notification:lab-results-ready"   → SMS + Telegram

Delayed Jobs (Kechiktirilgan):
- booking-confirmed: delay = 0 (darhol)
- reminder-3d:       delay = (appointment_time - 3 days)
- reminder-24h:      delay = (appointment_time - 24 hours)
- reminder-2h:       delay = (appointment_time - 2 hours)
- post-visit-review: delay = (appointment_time + 1 hour)
```

---

## 1.5. Bron Bekor Qilish va Ko'chirish

### 1.5.1. Bekor Qilish Siyosati

```
CANCELLATION POLICY:

┌────────────────────────────────────────────────────┐
│  QACHON BEKOR QILINSA           NATIJA             │
├────────────────────────────────────────────────────┤
│  24 soatdan ko'p oldin      │  100% pul qaytarish │
│  12-24 soat oldin           │  50% pul qaytarish  │
│  12 soatdan kam oldin       │  Pul qaytarilmaydi  │
│  Kelmaslik (No-show)        │  Pul qaytarilmaydi  │
│                             │  + "Yellow flag"     │
└────────────────────────────────────────────────────┘

3 marta "No-show" qilgan bemor:
→ Keyingi bron uchun 100% oldindan to'lov majburiy
→ Admin ogohlantirish oladi
```

### 1.5.2. Vaqt Ko'chirish (Reschedule)

```
KO'CHIRISH OQIMI:

1. Bemor "Ko'chirish" tugmasini bosadi
   │
2. Tizim avvalgi slotni AVAILABLE ga qaytaradi
   │
3. Yangi sana/vaqt tanlash ekrani ochiladi
   │  (avvalgi shifokor avtomatik tanlangan)
   │
4. Yangi slot tanlanadi va tasdiqlanadi
   │  (OTP qayta talab qilinmaydi — session'dan foydalaniladi)
   │
5. To'lov farqi bo'lsa → avtomatik hisoblash:
   │  ├── Arzonroq → farq qaytariladi
   │  └── Qimmatroq → qo'shimcha to'lov so'raladi
   │
6. Yangi tasdiqlash SMS/Telegram yuboriladi
   
CHEKLOV: Eng ko'pi bilan 2 marta ko'chirish mumkin.
3-marta ko'chirishda yangi bron qilish kerak.
```

---

## 1.6. Navbat UI/UX

### 1.6.1. Kalendar Komponenti

```
┌──────────────────────────────────────────────────────────┐
│   📅 Dr. Aliyev Xasan — Kardiolog                       │
│   ─────────────────────────────────                      │
│                                                          │
│          ◀ MART 2026 ▶                                   │
│   Du   Se   Chor  Pay  Ju   Sha  Yak                    │
│                              1    2                      │
│   3    4    5     6    7    8    9                       │
│   10   11   12    13   14  [15]  16                     │
│   17   18   19    20   21   22   23                     │
│   24   25   26    27   28   29   30                     │
│   31                                                     │
│                                                          │
│   [15] = Tanlangan kun (highlighted)                     │
│   Kulrang = band / o'tgan kunlar                         │
│   Yashil = bo'sh slotlari bor                           │
│                                                          │
│   15-MART UCHUN BO'SH VAQTLAR:                          │
│   ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐                  │
│   │09:00 │ │10:00 │ │11:00 │ │12:00 │                  │
│   │  ✅  │ │  ✅  │ │  ✅  │ │  ✅  │                  │
│   └──────┘ └──────┘ └──────┘ └──────┘                  │
│   ┌──────┐ ┌──────┐ ┌──────┐                            │
│   │14:00 │ │15:30 │ │16:00 │                            │
│   │  ✅  │ │  ✅  │ │  ✅  │                            │
│   └──────┘ └──────┘ └──────┘                            │
│                                                          │
│   Qabul narxi: 80,000 so'm                              │
│   Davomiyligi: 30 daqiqa                                 │
│                                                          │
│   ┌────────────────────────────────────┐                 │
│   │     [📅 10:00 NI TANLASH]          │                 │
│   └────────────────────────────────────┘                 │
└──────────────────────────────────────────────────────────┘
```

---

## 1.7. CRM Integratsiya

### 1.7.1. CRM ga Avtomatik Yozish

```
BRON TASDIQLANGANDA CRM GA YOZILADIGAN MA'LUMOTLAR:

{
  "booking_id": "BK-2026-0415-001",
  "patient": {
    "id": "PAT-00001234",           // Yangi bemor → avtomatik yaratiladi
    "full_name": "Ahmadov Sherzod",
    "phone": "+998901234567",
    "birth_date": "1990-05-15",
    "source": "WEBSITE_AI"          // Qayerdan keldi (AI, Direct, Telegram)
  },
  "appointment": {
    "doctor_id": "DOC-K-001",
    "department": "Kardiologiya",
    "date": "2026-03-15",
    "time": "10:00",
    "duration_min": 30,
    "type": "FIRST_VISIT",          // FIRST_VISIT | FOLLOW_UP | ONLINE
    "status": "CONFIRMED"
  },
  "payment": {
    "total": 80000,
    "paid": 16000,
    "method": "CLICK",
    "transaction_id": "CLK-2026031509001234",
    "status": "PARTIAL"             // FULL | PARTIAL | PENDING | CASH
  },
  "ai_context": {
    "symptoms": "Bosh og'rig'i, ko'ngil aynish",
    "ai_recommendation": "Nevrologiya (87%)",
    "session_id": "AI-SES-001234"
  },
  "notifications": {
    "sms_sent": true,
    "telegram_sent": true,
    "reminder_3d_scheduled": true,
    "reminder_24h_scheduled": true
  }
}
```

---

*Keyingi bo'lim: [04_BEMOR_SHAXSIY_KABINETI.md](./04_BEMOR_SHAXSIY_KABINETI.md)*
