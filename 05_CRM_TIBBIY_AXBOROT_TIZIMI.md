# 05. CRM Tibbiy Axborot Tizimi (MIS — Medical Information System)

> **Modul:** CRM/MIS Backend + Admin Dashboard  
> **Texnologiya:** NestJS, Prisma ORM, PostgreSQL, Redis, MinIO  
> **Mas'ul:** Backend Lead + Full-Stack Developer  

---

## 1.1. Elektron Tibbiy Karta (EHR — Electronic Health Record)

### 1.1.1. Bemor Kartochkasi Tuzilishi

```
BEMOR KARTOCHKASI ARXITEKTURASI:

┌────────────────────────────────────────────────────────────┐
│  👤 BEMOR KARTOCHKASI — #PAT-00001234                      │
│  ══════════════════════════════════════                     │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  📋 SHAXSIY MA'LUMOTLAR                              │   │
│  │  Ism: Ahmadov Sherzod Rustamovich                    │   │
│  │  Tug'ilgan: 15.05.1990 (35 yosh)                   │   │
│  │  Jinsi: Erkak                                        │   │
│  │  Telefon: +998 90 123-45-67                          │   │
│  │  Manzil: Toshkent, Chilonzor, 19-mavze              │   │
│  │  Qon guruhi: A(II) Rh+                              │   │
│  │  Allergiyalar: Penitsillin 🔴 OGOHLANTIRISH          │   │
│  │  Surunkali kasalliklar: Gipertoniya (2019-dan beri)  │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  📊 VITAL BELGILAR (Oxirgi kiritilgan)               │   │
│  │  Qon bosimi:   130/85 mmHg  (🟡 biroz yuqori)       │   │
│  │  Puls:          78 bpm      (✅ norma)               │   │
│  │  Harorat:       36.6°C      (✅ norma)               │   │
│  │  Vazn:          82 kg       SpO2: 98%               │   │
│  │  Bo'y:          175 sm      BMI: 26.8 (ortiqcha)    │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
│  TABS: [Tashrif tarix] [Tashxislar] [Tahlillar]            │
│        [Retseptlar] [Rasmlar] [Grafik]                     │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │  📜 TASHRIF TARIXI                                   │   │
│  │                                                      │   │
│  │  📅 10-Mart, 2026 — Dr. Aliyev (Kardiolog)          │   │
│  │  ├── Shikoyat: Bosh og'rig'i, yurak urishi          │   │
│  │  ├── Tashxis: I10 — Esensial gipertoniya            │   │
│  │  ├── Tahlillar: CBC, Bioximiya                       │   │
│  │  ├── Retsept: Amlodipine 5mg, Lisinopril 10mg      │   │
│  │  └── Keyingi tashrif: 24-Mart, 2026                 │   │
│  │                                                      │   │
│  │  📅 15-Yanvar, 2026 — Dr. Karimova (Terapevt)       │   │
│  │  ├── Shikoyat: Umumiy holsizlik                     │   │
│  │  ├── Tashxis: Z00.0 — Profilaktik tekshiruv         │   │
│  │  ├── Tahlillar: CBC                                  │   │
│  │  └── Status: YAKUNLANGAN                             │   │
│  │                                                      │   │
│  │  [Ko'proq yuklash...]                                │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└────────────────────────────────────────────────────────────┘
```

### 1.1.2. MKB-10 Integratsiya

```
MKB-10 QIDIRUV TIZIMI:

Shifokor "Tashxis" maydoniga yozganda:

Input: "giper..."
│
▼ Autocomplete natijalar:
┌─────────────────────────────────────────────────┐
│  I10   — Esensial (birlamchi) gipertoniya       │
│  I11   — Gipertoniyali yurak kasalligi          │
│  I12   — Gipertoniyali buyrak kasalligi         │
│  I13   — Gipertoniyali yurak va buyrak kasall.  │
│  I15   — Ikkilamchi gipertoniya                 │
│  E10   — 1-turdagi qandli diabet (boshqa)       │
└─────────────────────────────────────────────────┘

QIDIRUV IMKONIYATLARI:
- Kod bo'yicha: "I10" → to'g'ridan-to'g'ri topiladi
- Nom bo'yicha (o'zbekcha): "gipertoniya" → tegishli kodlar
- Nom bo'yicha (ruscha): "гипертония" → tegishli kodlar  
- Simptom bo'yicha: "qon bosimi yuqori" → I10, I11, I15
```

### 1.1.3. Multimedia Vault (Tibbiy Rasmlar)

```
MULTIMEDIA ARXIV TUZILISHI:

┌────────────────────────────────────────────────────────────┐
│  📁 Tibbiy Rasmlar — Ahmadov Sherzod                       │
│                                                             │
│  📁 Rentgen/                                                │
│  │  ├── 📷 2026-03-10_chest_xray.dcm  (DICOM)             │
│  │  ├── 📷 2025-11-05_spine_xray.dcm                      │
│  │  └── 📷 2025-06-20_hand_xray.jpg                       │
│  │                                                          │
│  📁 UZI/                                                    │
│  │  ├── 📷 2026-01-15_abdominal_uzi.jpg                   │
│  │  └── 📷 2025-09-10_thyroid_uzi.jpg                     │
│  │                                                          │
│  📁 MRT/                                                    │
│  │  └── 📷 2025-04-22_brain_mrt.dcm  (DICOM)              │
│  │                                                          │
│  📁 Laboratoriya PDF/                                       │
│     ├── 📄 2026-03-10_CBC_results.pdf                      │
│     ├── 📄 2026-01-15_biochemistry.pdf                     │
│     └── 📄 2025-11-20_hormone_panel.pdf                    │
│                                                             │
│  SAQLASH:                                                   │
│  ├── MinIO / AWS S3 (Object Storage)                       │
│  ├── DICOM fayllar uchun Orthanc server (ixtiyoriy)        │
│  ├── Har bir fayl AES-256 bilan shifrlangan                │
│  └── Faqat tegishli shifokor va bemor o'zi ko'ra oladi     │
│                                                             │
│  DICOM VIEWER:                                              │
│  ├── Cornerstone.js (Web-based viewer)                     │
│  ├── Zoom, rotate, contrast o'zgartirish                   │
│  ├── Measurement tools (o'lchash asboblari)                │
│  └── Side-by-side comparison (yonma-yon solishtirish)      │
└────────────────────────────────────────────────────────────┘
```

---

## 1.2. Shifokor Ish Joyi (Doctor Dashboard)

### 1.2.1. Shifokor Dashboard Wireframe

```
┌──────────────────────────────────────────────────────────────┐
│  🏥 Med-Core CRM    👨‍⚕️ Dr. Aliyev Xasan (Kardiolog)  [🔔] │
├──────────┬───────────────────────────────────────────────────┤
│          │                                                    │
│ 📋 Menyu │  📅 BUGUN: 10-MART, 2026 (Dushanba)              │
│          │  ─────────────────────────────                    │
│ 🏠 Bosh  │                                                    │
│ 📅 Jadval │  ┌───────────────────────────────────────────┐   │
│ 👥 Bemor │  │  BUGUNGI NAVBATLAR                         │   │
│ 💊 Retspt│  │                                            │   │
│ 📊 Stat  │  │  09:00 ✅ Ahmadov Sh. — Takroriy ko'rik   │   │
│ 📁 Shabln│  │  09:30 🔴 Karimova N. — Birinchi qabul    │   │
│ ⚙️ Sozlam│  │  10:00 🟢 Bo'sh                           │   │
│          │  │  10:30 🟡 Saidov A. — Onlayn (Telegram)    │   │
│          │  │  11:00 ✅ To'rayev B. — EKG natijasi        │   │
│          │  │  11:30 🟢 Bo'sh                            │   │
│          │  │  12:00 🔴 Rahimova D. — Dori tekshiruvi     │   │
│          │  │  ─── TUSHLIK ───                            │   │
│          │  │  14:00 ✅ Ergashev K. — Shikoyat takrori    │   │
│          │  │  ... (davomi)                               │   │
│          │  └───────────────────────────────────────────┘   │
│          │                                                    │
│          │  ┌────────────┐ ┌────────────┐ ┌────────────┐    │
│          │  │ 📊 Bugun   │ │ 📈 Bu hafta│ │ 🔔 Ogohlan.│    │
│          │  │ 8 bemor    │ │ 32 bemor   │ │ 2 ta yangi │    │
│          │  │ 2 onlayn   │ │ Reyting:4.9│ │ lab natija │    │
│          │  └────────────┘ └────────────┘ └────────────┘    │
│          │                                                    │
└──────────┴───────────────────────────────────────────────────┘
```

### 1.2.2. Qabul Jarayoni (Visit Workflow)

```
SHIFOKOR QABUL ALGORITMI:

1. Bemor keldi → Registratura "Keldi" tugmasini bosadi
   │
2. Shifokor dashboardida status: 🟢 → 🔴 "Kutish zalida"
   │
3. Shifokor "Qabul Boshlash" tugmasini bosadi
   │  ┌─────────────────────────────────────────────────────┐
   │  │  Timer boshlanadi: 00:00 → (qabul davomiyligi)     │
   │  │  Bemorning BARCHA tarixi ekranda ochiladi           │
   │  └─────────────────────────────────────────────────────┘
   │
4. Shifokor kiritadi:
   │  ├── 📝 Shikoyatlar (anamnez) — matn maydoni
   │  ├── 🔍 Tekshiruv natijalari — matn + shablon
   │  ├── 🏥 Tashxis — MKB-10 autocomplete
   │  ├── 💊 Retsept — dorilar ro'yxati (shablon + qo'lda)
   │  ├── 🔬 Tahlillar buyurtmasi — laboratoriyaga yuboriladi
   │  ├── 📸 Rasmlar — yuklash (rentgen, UZI)
   │  └── 📅 Keyingi tashrif — sana belgilash (avtomatik bron)
   │
5. "Qabul Yakunlash" tugmasi bosiladi
   │
6. Avtomatik ravishda:
   ├── Barcha ma'lumotlar EHR ga saqlanadi
   ├── Retsept bemorga SMS/Telegram yuboriladi
   ├── Tahlil buyurtmasi laboratoriyaga tushadi
   ├── Keyingi tashrif uchun slot bronlanadi
   ├── Ombordagi sarflangan materiallar chiqariladi
   └── To'lov kassaga yoziladadi
```

### 1.2.3. Smart Shablonlar (Templates)

```
SHABLON TIZIMI:

Shifokor har safar qaytadan yozmasligi uchun tayyor shablonlar:

┌─────────────────────────────────────────────────────────────┐
│  📁 SHABLONLAR KUTUBXONASI                                   │
│                                                               │
│  ┌── 🏥 Tashxis Shablonlari                                 │
│  │   ├── "Esensial gipertoniya, 2-daraja, yuqori xavf"      │
│  │   ├── "ORVI, yengil kechishi, asoratlanmagan"             │
│  │   ├── "Oshqozon yarasi, HP-pozitiv, birinchi aniqlangan"  │
│  │   └── [+ Yangi shablon yaratish]                          │
│  │                                                            │
│  ├── 💊 Dori Shablonlari (Davolash Sxemalari)                │
│  │   ├── "Gipertoniya standart": Amlodipine 5mg + Lisinopril │
│  │   ├── "ORVI standart": Paracetamol + Vitamin C            │
│  │   ├── "Antibiotik 7-kun": Amoxicillin 500mg x3            │
│  │   └── [+ Yangi sxema yaratish]                            │
│  │                                                            │
│  ├── 🔬 Tahlil Buyurtma Shablonlari                          │
│  │   ├── "Umumiy tekshiruv": CBC + Bioximiya + Siydik        │
│  │   ├── "Kardiologik": Lipid profil + EKG + ExoKG           │
│  │   ├── "Endokrin": TSH + T3 + T4 + Qand                   │
│  │   └── [+ Yangi shablon]                                    │
│  │                                                            │
│  └── 📝 Tekshiruv Shablonlari                                │
│      ├── "Umumiy ko'rik": Teri rangi, shilliq qavatlar...    │
│      ├── "Kardiologik": Yurak tonlari, qon bosimi...         │
│      └── [+ Yangi shablon]                                    │
│                                                               │
│  ⚡ SMART AUTOCOMPLETE:                                       │
│  Shifokor yoza boshlaganda, shablonlardan mos variant         │
│  taklif qilinadi. Tab tugmasi bilan qabul qilish.            │
└─────────────────────────────────────────────────────────────┘
```

---

## 1.3. Laboratoriya Integratsiya

### 1.3.1. Tahlil Buyurtmasi → Natija Oqimi

```
LABORATORIYA WORKFLOW:

┌──────────┐    ┌──────────────┐    ┌──────────────┐    ┌──────────┐
│ SHIFOKOR │───▶│ TIZIM        │───▶│ LABORATORIYA │───▶│ BEMOR    │
│          │    │              │    │              │    │          │
│ Tahlil   │    │ Buyurtma     │    │ Natija       │    │ Shaxsiy  │
│ buyuradi │    │ yaratadi     │    │ kiritadi     │    │ kabinetda│
│          │    │              │    │              │    │ ko'radi  │
└──────────┘    └──────────────┘    └──────────────┘    └──────────┘
      │                │                   │                  │
      ▼                ▼                   ▼                  ▼
  [Shablon]      [Lab ga xabar]    [Auto-validation]  [AI sharh + PDF]
                  [shtrix-kod]      [normadan og'ish   [push bildir.]
                                    aniqlash]

BUYURTMA MA'LUMOTLARI:
{
  "order_id": "LAB-2026-00456",
  "patient_id": "PAT-00001234",
  "doctor_id": "DOC-K-001",
  "tests": [
    {"code": "CBC", "name": "Umumiy Qon Tahlili", "priority": "ROUTINE"},
    {"code": "BIO", "name": "Bioximik Tahlil", "priority": "ROUTINE"}
  ],
  "notes": "Och qoringa, ertalab 08:00 gacha",
  "status": "ORDERED",
  "barcode": "2026031000456"     // Lab uchun shtrix-kod
}

STATUS BOSQICHLARI:
ORDERED → SAMPLE_COLLECTED → IN_PROGRESS → COMPLETED → VALIDATED → DELIVERED
```

---

## 1.4. Inventar va Ombor Boshqaruvi

### 1.4.1. Ombor Tuzilishi

```
OMBOR ARXITEKTURASI:

┌────────────────────────────────────────────────────────────────┐
│  📦 OMBOR BOSHQARUVI                                          │
│                                                                │
│  ┌──────────────────────────────────────────────────────────┐ │
│  │  KATEGORIYALAR:                                           │ │
│  │                                                           │ │
│  │  💊 Dori-darmonlar          │  🩹 Sarflanadigan materiallar│ │
│  │  ├── Tabletka              │  ├── Shpritslar              │ │
│  │  ├── Ampulalar             │  ├── Qo'lqoplar              │ │
│  │  ├── Suspenziyalar          │  ├── Binlar                  │ │
│  │  └── Tashqi dorilar         │  └── Dezinfektantlar         │ │
│  │                             │                               │ │
│  │  🔧 Tibbiy uskunalar        │  🧪 Lab reagentlar           │ │
│  │  ├── Stetoskoplar          │  ├── Qon tahlil reagentlari  │ │
│  │  ├── Termometrlar          │  ├── Siydik tahlil stiplari  │ │
│  │  └── Manometrlar           │  └── Diagnostik kitlar       │ │
│  │                                                           │ │
│  └──────────────────────────────────────────────────────────┘ │
│                                                                │
│  📊 OMBOR HOLATI (REAL-VAQT):                                 │
│  ┌──────────────┬──────┬──────┬──────────┬─────────────────┐  │
│  │ Nomi          │Qoldi │Min   │Muddati   │ Status          │  │
│  ├──────────────┼──────┼──────┼──────────┼─────────────────┤  │
│  │ Amlodipine 5mg│ 450  │ 100  │ 2027-06  │ ✅ Yetarli      │  │
│  │ Shprits 5ml   │ 85   │ 200  │ —        │ 🔴 Kam! Buyurt. │  │
│  │ Lidokain 2%   │ 30   │ 50   │ 2026-04  │ ⚠️ Muddati yaqn │  │
│  │ Qo'lqop L     │ 500  │ 100  │ —        │ ✅ Yetarli      │  │
│  │ Paracetamol   │ 200  │ 150  │ 2028-01  │ ✅ Yetarli      │  │
│  └──────────────┴──────┴──────┴──────────┴─────────────────┘  │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

### 1.4.2. Auto-Deduction (Avtomatik Chiqarish) Algoritmi

```
AVTOMATIK CHIQARISH OQIMI:

1. Shifokor qabul yakunlaydi:
   │  Tashxis: I10 — Gipertoniya
   │  Muolaja: EKG + Qabul
   │
2. Tizim "Muolaja ↔ Material" jadvalini tekshiradi:
   │
   │  ┌──────────────────┬──────────────────────────┐
   │  │  Muolaja          │  Kerakli materiallar      │
   │  ├──────────────────┼──────────────────────────┤
   │  │  Birinchi qabul   │  Qo'lqop x2, Spirt x1   │
   │  │  EKG              │  Elektrod x10, Gel x1    │
   │  │  In'ektsiya       │  Shprits x1, Qo'lqop x2 │
   │  │  Jarrohlik        │  To'liq operatsion kit   │
   │  └──────────────────┴──────────────────────────┘
   │
3. Ombordan avtomatik chiqariladi:
   │  ├── Qo'lqop (L): 500 → 498
   │  ├── EKG elektrodlar: 890 → 880
   │  └── EKG gel: 45 → 44
   │
4. Agar material minimum miqdordan kam bo'lsa:
   │  → Omborchi va Admin'ga ogohlantirish yuboriladi
   │  → Avtomatik buyurtma taklifi shakllantiriladi
   │
5. Har oyda: Material sarfi hisoboti generatsiya qilinadi
```

### 1.4.3. Muddati O'tayotgan Dorilar Ogohlantirishi

```
EXPIRY MONITORING:

CRON JOB: Har kuni kechasi 02:00 da tekshiriladi

Qoidalar:
├── 3 oy qolsa  → 🟡 Informatsion (Admin uchun)
├── 1 oy qolsa  → 🟠 Ogohlantirish (Admin + Omborchi)
├── 1 hafta qolsa → 🔴 Kritik (Zudlik bilan chora)
└── Muddati o'tgan → ⛔ Foydalanishdan chiqariladi + Hisobot

Xabar namunasi:
"⚠️ Ogohlantirish: Lidokain 2% (30 dona) muddati 
2026-04-15 da tugaydi. Qolgan: 37 kun. 
[📋 Almashtirish buyurtmasi] [🗑️ Utilizatsiya qilish]"
```

---

## 1.5. Elektron Retsept Tizimi

### 1.5.1. Retsept Yaratish Oqimi

```
E-RETSEPT ALGORITMI:

1. Shifokor dori tanlaydi (autocomplete):
   │  Input: "amlo..."
   │  → Amlodipine 5mg | 10mg
   │  → Amlodipine + Valsartan 5/160mg
   │
2. Dozaj va sxema belgilaydi:
   │  ├── Doza: 5 mg
   │  ├── Kuniga: 1 marta
   │  ├── Qachon: Ertalab
   │  ├── Davomiylik: 30 kun
   │  └── Izoh: "Ovqatdan keyin"
   │
3. Tizim avtomatik tekshiradi:
   │  ├── ⚠️ Allergiya tekshiruvi: 
   │  │   → Bemor "Penitsillin"ga allergik
   │  │   → Agar tanlangan dori penitsillinga oid → BLOKLASH
   │  │
   │  ├── ⚠️ Dori-dori o'zaro ta'siri:
   │  │   → Bemor allaqachon Lisinopril ichyapti
   │  │   → Agar ijobiy ta'sir → OK
   │  │   → Agar salbiy ta'sir → OGOHLANTIRISH
   │  │
   │  └── ⚠️ Dozaj tekshiruvi:
   │      → Yosh, vazn va tashxisga qarab ruxsat etilgan dozaj
   │
4. Retsept tasdiqlanadi va saqlanadi
   │
5. Bemorga yuboriladi:
   ├── SMS: "Dori retseptingiz tayyor. Shaxsiy kabinetdan ko'ring."
   ├── Telegram: PDF + QR kod
   └── Bemor portali: Ko'rish + yuklab olish
```

---

## 1.6. Registratura Moduli

### 1.6.1. Registratura Dashboard

```
┌──────────────────────────────────────────────────────────────┐
│  🏥 REGISTRATURA                  👩 Malika (Registrator)    │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  📊 BUGUNGI STATISTIKA:                                     │
│  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐             │
│  │  42  │ │  35  │ │   5  │ │   2  │ │  🔴  │             │
│  │Jami  │ │Kelgan│ │Kutish│ │Kecik.│ │Kelma.│             │
│  └──────┘ └──────┘ └──────┘ └──────┘ └──────┘             │
│                                                              │
│  📋 HOZIRGI NAVBAT (Real-vaqt):                             │
│  ┌───┬───────────────┬──────────┬────────┬─────────────┐   │
│  │ # │ Bemor          │ Shifokor │ Vaqt   │ Status      │   │
│  ├───┼───────────────┼──────────┼────────┼─────────────┤   │
│  │ 1 │ Ahmadov Sh.    │ Dr.Aliyev│ 09:00  │ 🔵 Qabuldа │   │
│  │ 2 │ Karimova N.    │ Dr.Aliyev│ 09:30  │ 🟡 Kutmoqda│   │
│  │ 3 │ Saidov A.      │ Dr.Aliyev│ 10:00  │ ⬜ Kelmagan │   │
│  │ 4 │ To'rayev B.    │ Dr.Said  │ 09:00  │ 🟢 Keldi   │   │
│  └───┴───────────────┴──────────┴────────┴─────────────┘   │
│                                                              │
│  [➕ Yangi bemor qo'shish]  [🔍 Bemor qidirish]             │
│  [📞 SMS yuborish]          [🖨️ Navbat chop etish]          │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

*Keyingi bo'lim: [06_MOLIYA_VA_ADMIN_PANEL.md](./06_MOLIYA_VA_ADMIN_PANEL.md)*
