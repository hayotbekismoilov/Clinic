# 06. Moliya va Admin Panel

> **Modul:** Financial Management + Admin Dashboard  
> **Texnologiya:** NestJS, PostgreSQL, Click/Payme SDK, Chart.js/Recharts  
> **Mas'ul:** Backend Developer + Finance Consultant  

---

## 1.1. To'lov Integratsiyasi

### 1.1.1. Qo'llab-Quvvatlanadigan To'lov Usullari

```
TO'LOV KANALLARI:

┌──────────────────────────────────────────────────────────────┐
│                    TO'LOV EKOTIZIMI                           │
│                                                               │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│  │  CLICK   │  │  PAYME   │  │  NAQD    │  │ KARTA    │    │
│  │  (API)   │  │  (API)   │  │ (Kassa)  │  │ (POS)    │    │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘    │
│       │              │              │              │          │
│       └──────────────┼──────────────┼──────────────┘          │
│                      │              │                         │
│                      ▼              ▼                         │
│              ┌──────────────┐ ┌──────────────┐               │
│              │  ONLAYN      │ │  OFLAYN      │               │
│              │  TO'LOVLAR   │ │  TO'LOVLAR   │               │
│              └──────┬───────┘ └──────┬───────┘               │
│                     │                │                        │
│                     └────────┬───────┘                        │
│                              │                                │
│                     ┌────────▼────────┐                      │
│                     │  UNIFIED BILLING │                      │
│                     │  (Yagona hisob)  │                      │
│                     └────────┬────────┘                       │
│                              │                                │
│                     ┌────────▼────────┐                      │
│                     │  MOLIYA HISOBOTI │                      │
│                     │  (Dashboard)     │                      │
│                     └─────────────────┘                       │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

### 1.1.2. Click Integratsiya

```
CLICK API INTEGRATSIYASI:

1. Bemor "Click orqali to'lash" tugmasini bosadi
   │
2. Server Click API ga so'rov yuboradi:
   POST https://api.click.uz/v2/merchant/invoice/create
   {
     "service_id": 12345,
     "merchant_id": 67890,
     "amount": 80000,
     "merchant_trans_id": "BK-2026-0415-001",
     "return_url": "https://klinika.uz/booking/success"
   }
   │
3. Bemor Click ilovasida to'lovni tasdiqlaydi
   │
4. Click callback: POST /api/v1/payments/click/callback
   {
     "click_trans_id": 123456789,
     "merchant_trans_id": "BK-2026-0415-001",
     "amount": 80000,
     "action": 1,           // 0=prepare, 1=complete
     "error": 0,
     "sign_time": "2026-03-09 13:45:00"
   }
   │
5. Server to'lovni tasdiqlaydi:
   ├── Bron statusi: PAYMENT_PENDING → CONFIRMED
   ├── Molliya bazasiga yoziladi
   └── Bemorga "To'lov muvaffaqiyatli" xabari yuboriladi
```

### 1.1.3. Payme Integratsiya

```
PAYME API INTEGRATSIYASI:

JSONRPC-based:

1. Invoice yaratish:
   POST https://checkout.paycom.uz/api
   {
     "method": "receipts.create",
     "params": {
       "amount": 8000000,    // tiyinlarda (80,000 so'm = 8,000,000 tiyin)
       "account": {
         "booking_id": "BK-2026-0415-001"
       }
     }
   }

2. Payme Callback:
   POST /api/v1/payments/payme/callback
   Methods: CreateTransaction, PerformTransaction, 
            CancelTransaction, CheckPerformTransaction,
            GetStatement

3. Xavfsizlik:
   ├── Basic Auth header: Authorization: Basic <base64(id:key)>
   ├── IP whitelist: Faqat Payme serverlaridan
   └── Signature verification: SHA-256
```

### 1.1.4. "Zaklad" (Avans) Anti-Fake Tizimi

```
AVANS TO'LOV SIYOSATI:

MAQSAD: "Feyk" navbatlarni kamaytirish. Bemor oldindan to'lov qilsa,
         ishonch darajasi oshadi va no-show kamayadi.

SIYOSAT:
┌────────────────────────────────────────────────────────────┐
│  Xizmat turi          │ Avans miqdori │ Qachon to'lanadi   │
├────────────────────────┼───────────────┼────────────────────┤
│  Birinchi qabul        │ 20% (16,000)  │ Bron paytida       │
│  Qayta qabul           │ 0% (bepul)    │ Klinikada          │
│  Operatsiya            │ 30% min       │ Bron paytida       │
│  Onlayn konsultatsiya  │ 100%          │ Bron paytida       │
│  VIP qabul             │ 50%           │ Bron paytida       │
└────────────────────────┴───────────────┴────────────────────┘

QAYTARISH SIYOSATI:
├── 24 soatdan ko'p oldin bekor → 100% qaytarish
├── 12-24 soat oldin → 50% qaytarish
├── 12 soatdan kam → Qaytarilmaydi
└── 3+ no-show → Keyingi bron 100% prepaid
```

---

## 1.2. Admin Panel (Super Dashboard)

### 1.2.1. Asosiy Dashboard

```
┌──────────────────────────────────────────────────────────────┐
│  🏥 MED-CORE ADMIN PANEL           👤 Admin  📅 10-Mart     │
├──────────┬───────────────────────────────────────────────────┤
│          │                                                    │
│ 📊 Dash  │  📊 UMUMIY KO'RSATKICHLAR                        │
│ 👥 Bemor │  ─────────────────────                            │
│ 👨‍⚕️ Shift│                                                   │
│ 💰 Moliya│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────┐│
│ 📦 Ombor │  │  💰       │ │  👥       │ │  📅       │ │ ⭐   ││
│ 🤖 AI Log│  │ 12.5M     │ │  342      │ │  48       │ │ 4.7  ││
│ ⚙️ Sozl. │  │ Oylik     │ │ Bemorlar │ │ Bugungi  │ │ O'rt.││
│          │  │ Tushum    │ │ (bu oy)  │ │ navbatlar│ │Reytin││
│          │  │ ↑12%      │ │ ↑8%      │ │          │ │      ││
│          │  └──────────┘ └──────────┘ └──────────┘ └──────┘│
│          │                                                    │
│          │  📈 KUNLIK TUSHUM GRAFIGI (Oxirgi 30 kun):        │
│          │  800K─┤          ╱╲                                │
│          │  600K─┤    ╱╲  ╱    ╲   ╱╲                        │
│          │  400K─┤  ╱    ╲╱      ╲╱  ╲────                   │
│          │  200K─┤╱                                            │
│          │       └──┬──┬──┬──┬──┬──┬──┬──                    │
│          │         10 15 20 25 1  5  10                       │
│          │                                                    │
│          │  🏆 TOP SHIFOKORLAR (Tushum bo'yicha):            │
│          │  1. Dr. Aliyev    — 2.1M so'm  │ ⭐4.9 │ 45 bem. │
│          │  2. Dr. Karimova  — 1.8M so'm  │ ⭐4.8 │ 52 bem. │
│          │  3. Dr. Saidov    — 1.5M so'm  │ ⭐4.7 │ 38 bem. │
│          │                                                    │
│          │  🔥 ENG OMMALASHGAN YO'NALISHLAR:                  │
│          │  1. Terapiya ─────── 28% ████████████░░░           │
│          │  2. Kardiologiya ─── 18% ████████░░░░░░            │
│          │  3. Stomatologiya ── 15% ██████░░░░░░░             │
│          │  4. Nevrologiya ──── 12% █████░░░░░░░░             │
│          │  5. Ginekologiya ─── 10% ████░░░░░░░░░             │
│          │                                                    │
└──────────┴───────────────────────────────────────────────────┘
```

### 1.2.2. Shifokorlar KPI Tizimi

```
KPI METRIKALARI:

┌──────────────────────────────────────────────────────────────┐
│  📊 SHIFOKOR KPI — Dr. Aliyev Xasan (Kardiolog)             │
│                                                               │
│  ASOSIY KO'RSATKICHLAR:                                      │
│  ┌────────────────────────┬──────────┬──────────┬──────────┐ │
│  │ Metrika                │ Bu oy    │ O'tgan oy│ O'sish   │ │
│  ├────────────────────────┼──────────┼──────────┼──────────┤ │
│  │ Qabul qilgan bemorlar  │ 45       │ 42       │ ↑ 7%     │ │
│  │ O'rtacha qabul vaqti   │ 22 min   │ 25 min   │ ↓ 12%   │ │
│  │ Bemor reytingi         │ 4.9/5    │ 4.8/5    │ ↑ 2%     │ │
│  │ Qayta kelish (30 kun)  │ 68%      │ 65%      │ ↑ 4.6%  │ │
│  │ No-show rate           │ 5%       │ 8%       │ ↓ 37%   │ │
│  │ Jami tushum            │ 2.1M     │ 1.9M     │ ↑ 10%   │ │
│  │ O'rtacha chek          │ 46,667   │ 45,238   │ ↑ 3%    │ │
│  └────────────────────────┴──────────┴──────────┴──────────┘ │
│                                                               │
│  BEMORLAR FIKRLARI:                                          │
│  ⭐⭐⭐⭐⭐ "Juda yaxshi shifokor, hamma narsani tushuntirdi" │
│  ⭐⭐⭐⭐  "Yaxshi, lekin kutishdim biroz"                    │
│  ⭐⭐⭐⭐⭐ "15 yillik muammomni hal qildi!"                  │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

### 1.2.3. Moliya Hisobotlari

```
MOLIYA DASHBOARD:

┌──────────────────────────────────────────────────────────────┐
│  💰 MOLIYA HISOBOTI — MART 2026                              │
│                                                               │
│  ┌───────────────────────────────────────────────────────┐   │
│  │  TUSHUM TARKIBI:                                      │   │
│  │                                                       │   │
│  │  Click:    4,200,000 so'm   ██████████████░░  (33.6%)│   │
│  │  Payme:    3,800,000 so'm   ████████████░░░░  (30.4%)│   │
│  │  Naqd:     3,000,000 so'm   ██████████░░░░░░  (24.0%)│   │
│  │  Karta:    1,500,000 so'm   █████░░░░░░░░░░░  (12.0%)│   │
│  │  ─────────────────────────────────────────────────    │   │
│  │  JAMI:    12,500,000 so'm                             │   │
│  └───────────────────────────────────────────────────────┘   │
│                                                               │
│  ┌───────────────────────────────────────────────────────┐   │
│  │  YO'NALISH BO'YICHA TUSHUM:                           │   │
│  │                                                       │   │
│  │  Kardiologiya:    3,500,000  (28%)                    │   │
│  │  Stomatologiya:   2,800,000  (22.4%)                  │   │
│  │  Nevrologiya:     1,600,000  (12.8%)                  │   │
│  │  Terapiya:        1,200,000  (9.6%)                   │   │
│  │  Boshqalar:       3,400,000  (27.2%)                  │   │
│  └───────────────────────────────────────────────────────┘   │
│                                                               │
│  📥 EKSPORT: [PDF] [Excel] [1C Integratsiya]                 │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

---

## 1.3. AI Analytics (Biznes Intellekti)

### 1.3.1. AI Qidiruv Trendlari

```
AI INSIGHTS ENGINE:

Maqsad: Bemorlar ko'p qidirayotgan simptomlar tahlili orqali
        biznes qarorlar qabul qilishga yordam.

MISOL:
┌──────────────────────────────────────────────────────────┐
│  🤖 AI INSIGHTS — Mart 2026                             │
│                                                          │
│  📈 TREND #1: Allergiya so'rovlari 40% OSHDI            │
│  ├── O'tgan oy: 45 ta so'rov                            │
│  ├── Bu oy: 63 ta so'rov (+40%)                         │
│  ├── Sabab: Bahorda allergiya mavsumi                   │
│  └── 💡 TAVSIYA: Allergolog grafigini kengaytiring       │
│      va antihistamin dorilarni omborga buyurtma qiling.  │
│                                                          │
│  📈 TREND #2: "Bel og'rig'i" so'rovlari barqaror yuqori │
│  ├── Oylik o'rtacha: 35 ta so'rov                       │
│  ├── Hozirgi ortoped soni: 1 ta                         │
│  └── 💡 TAVSIYA: Ikkinchi ortoped yollash                │
│      haftasiga kamida 25 ta qo'shimcha bemor kutiladi.   │
│                                                          │
│  📈 TREND #3: "Onlayn konsultatsiya" talabi oshmoqda     │
│  ├── Bu oy: 89 ta so'rov (umumiyning 12%)               │
│  └── 💡 TAVSIYA: Telemedicina modulini kengaytiring.     │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

### 1.3.2. Retention (Qayta Chaqirish) Tizimi

```
AVTOMATIK QAYTA CHAQIRISH ALGORITMI:

CRON JOB: Har kuni ertalab 09:00

1. Tizim barcha bemorlarni tekshiradi:
   │
   ├── Profilaktik tekshiruv kerakmi?
   │   → Oxirgi umumiy tekshiruvdan 6 oy o'tdimi?
   │   → Ha → "Salom, olti oylik tekshiruv vaqti keldi!
   │            Navbatga yozilamizmi?" [Ha] [Yo'q]
   │
   ├── Takroriy qabul belgilangan-mi?
   │   → Dr. Aliyev "1 oydan keyin kontrolga keling" degan
   │   → 25 kun o'tdi → "Kontrol qabul vaqti yaqinlashmoqda.
   │            Dr. Aliyev'ga yozilamizmi?" [Ha] [Yo'q]
   │
   ├── Stomatologiya (6 oyda 1):
   │   → Tish tozalashdan 6 oy o'tdi
   │   → "Salom! Tishlaringizni profilaktik tekshiruv
   │      vaqti keldi. [Navbat olish]"
   │
   └── Dori kursi tugaganmi?
       → Retseptdagi 30 kunlik dori kursi tugadi
       → "Dori kursigiz tugadi. Shifokorga murojaat qiling.
          [Qayta yozilish]"

SAMARADORLIK:
- Qayta chaqirish conversion rate: ~25-35%
- Oylik qo'shimcha tushum: ~2-3 mln so'm (taxminiy)
```

---

## 1.4. Foydalanuvchilarni Boshqarish

### 1.4.1. Rol va Huquqlarni Boshqarish

```
RBAC MATRITSASI:

┌─────────────────┬────────┬────────┬────────┬────────┬────────┐
│ Modul            │ Admin  │Shifokor│Hamshira│Registr.│ Kassir │
├─────────────────┼────────┼────────┼────────┼────────┼────────┤
│ Bemor shaxsiy    │ ✅ R/W │ 🔶 R*  │ 🔶 R*  │ ✅ R/W │ ❌     │
│ Tibbiy tarix     │ ✅ R   │ ✅ R/W*│ 🔶 R*  │ ❌     │ ❌     │
│ Navbatlar        │ ✅ R/W │ ✅ R   │ ✅ R   │ ✅ R/W │ ❌     │
│ Retseptlar       │ ✅ R   │ ✅ R/W │ 🔶 R   │ ❌     │ ❌     │
│ Tahlillar        │ ✅ R   │ ✅ R/W │ 🔶 R   │ ❌     │ ❌     │
│ Moliya/To'lov    │ ✅ R/W │ 🔶 R   │ ❌     │ 🔶 R   │ ✅ R/W │
│ Ombor/Inventar   │ ✅ R/W │ ❌     │ ❌     │ ❌     │ ❌     │
│ KPI/Statistika   │ ✅ R/W │ 🔶 R*  │ ❌     │ ❌     │ ❌     │
│ Tizim sozlamalari│ ✅ R/W │ ❌     │ ❌     │ ❌     │ ❌     │
│ AI loglar        │ ✅ R   │ ❌     │ ❌     │ ❌     │ ❌     │
└─────────────────┴────────┴────────┴────────┴────────┴────────┘

Izohlar:
✅ R/W  = To'liq o'qish va yozish
✅ R    = Faqat o'qish
🔶 R*   = O'z bemorlarigagina (shifokor faqat o'z bemori)
🔶 R    = Cheklangan ko'rish
❌      = Ruxsat yo'q
```

---

*Keyingi bo'lim: [07_XAVFSIZLIK_VA_ARXITEKTURA.md](./07_XAVFSIZLIK_VA_ARXITEKTURA.md)*
