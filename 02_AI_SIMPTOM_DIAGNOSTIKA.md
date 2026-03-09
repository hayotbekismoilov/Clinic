# 02. AI-Simptom Diagnostika va Smart Qidiruv

> **Modul:** AI Diagnostics Engine  
> **Texnologiya:** LangChain, OpenAI GPT-4o / Llama-3, ChromaDB, spaCy  
> **Mas'ul:** AI/ML Engineer + Backend Developer  

---

## 1.1. Umumiy Konseptsiya

Bemor qaysi shifokorga borishni bilmasligi — tibbiy xizmatdagi eng katta muammo. Bu modul foydalanuvchiga **tabiiy tilda** simptomlarini yozish imkonini beradi va AI ularni tahlil qilib, to'g'ri yo'nalishga yo'naltiradi.

### 1.1.1. Qanday Ishlaydi? (Yuqori Daraja)

```
┌─────────────────────────────────────────────────────────────┐
│                  AI DIAGNOSTIKA OQIMI                       │
│                                                              │
│  👤 Bemor                                                    │
│  │                                                           │
│  ▼                                                           │
│  ┌────────────────────────────────────┐                      │
│  │  "Boshim og'riyapti va              │                      │
│  │   ko'nglim ayniyapti,              │                      │
│  │   ko'z oldim qorong'ilashyapti"    │                      │
│  └──────────────┬─────────────────────┘                      │
│                 │                                             │
│                 ▼                                             │
│  ┌────────────────────────────────────┐                      │
│  │  📝 NLP PRE-PROCESSING             │                      │
│  │  ├── Tokenizatsiya                 │                      │
│  │  ├── Simptomlarni ajratish         │                      │
│  │  ├── O'zbek → Tibbiy terminlar     │                      │
│  │  └── Anonimlash (PII removal)      │                      │
│  └──────────────┬─────────────────────┘                      │
│                 │                                             │
│                 ▼                                             │
│  ┌────────────────────────────────────┐                      │
│  │  🔍 RAG ENGINE                      │                      │
│  │  ├── Vector search (ChromaDB)      │                      │
│  │  ├── Tibbiy protokollar bazasi     │                      │
│  │  ├── MKB-10 simptom mapping        │                      │
│  │  └── Context retrieval             │                      │
│  └──────────────┬─────────────────────┘                      │
│                 │                                             │
│                 ▼                                             │
│  ┌────────────────────────────────────┐                      │
│  │  🤖 LLM INFERENCE                  │                      │
│  │  ├── GPT-4o / Llama-3             │                      │
│  │  ├── System prompt (tibbiy rol)   │                      │
│  │  ├── Retrieved context injection  │                      │
│  │  └── Structured output (JSON)     │                      │
│  └──────────────┬─────────────────────┘                      │
│                 │                                             │
│                 ▼                                             │
│  ┌────────────────────────────────────┐                      │
│  │  📊 NATIJA                          │                      │
│  │  ├── Taxminiy yo'nalish: Nevrolog  │                      │
│  │  ├── Ishonch darajasi: 87%         │                      │
│  │  ├── Muqobil: Terapevt (45%)      │                      │
│  │  ├── Top 3 shifokor               │                      │
│  │  └── [Navbatga yozilish] tugmasi  │                      │
│  └────────────────────────────────────┘                      │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 1.2. NLP Pre-Processing (Oldindan Qayta Ishlash)

### 1.2.1. O'zbek Tilini Qayta Ishlash

O'zbek tilida tibbiy sohaga ixtisoslashgan NLP modeli mavjud emasligi sababli, quyidagi yondashuv qo'llaniladi:

```
QAYTA ISHLASH BOSQICHLARI:

1. Matnni tozalash:
   Input:  "boshim qattiq ogriyapti!!! ko'zim oldim qorongilashyapti..."
   Output: "boshim qattiq og'riyapti ko'zim oldim qorong'ilashyapti"

2. Tokenizatsiya:
   Output: ["boshim", "qattiq", "og'riyapti", "ko'zim", "oldim", "qorong'ilashyapti"]

3. Simptom Entity Recognition:
   Output: [
     {"entity": "bosh og'rig'i", "original": "boshim qattiq og'riyapti", "confidence": 0.95},
     {"entity": "ko'z qorong'ilashi", "original": "ko'zim oldim qorong'ilashyapti", "confidence": 0.88}
   ]

4. Tibbiy terminlarga tarjima:
   Output: [
     {"medical_term": "Cephalgia", "mkb10": "R51", "symptom_uz": "bosh og'rig'i"},
     {"medical_term": "Scotoma", "mkb10": "H53.1", "symptom_uz": "ko'z qorong'ilashi"}
   ]
```

### 1.2.2. Simptomlar Lug'ati (O'zbekcha → Tibbiy)

Tizimda kamida **2000+** simptom-termin juftligi bo'ladi:

| O'zbekcha ifoda | Tibbiy termin | MKB-10 |
|----------------|---------------|--------|
| "Boshim og'riyapti" | Cephalgia | R51 |
| "Ko'nglim ayniyapti" | Nausea | R11 |
| "Haroratim ko'tarildi" | Pyrexia/Fever | R50 |
| "Yuragim tez urmoqda" | Tachycardia | R00.0 |
| "Nafas olishim qiyinlashdi" | Dyspnea | R06.0 |
| "Qornim og'riyapti" | Abdominal pain | R10 |
| "Belim og'riyapti" | Lumbago | M54.5 |
| "Ko'zim ko'rmay qoldi" | Amaurosis | H54 |
| "Terim qichiyapti" | Pruritus | L29 |
| "Qulog'im og'riyapti" | Otalgia | H92.0 |
| "Tomog'im og'riyapti" | Pharyngitis | J02 |
| "Burun oqyapti" | Rhinorrhea | J30.0 |
| "Chanqayapman" | Polydipsia | R63.1 |
| "Vaznim oshdi" | Weight gain | R63.5 |
| *... (2000+ ta)* | | |

---

## 1.3. RAG (Retrieval-Augmented Generation) Pipeline

### 1.3.1. RAG Arxitekturasi

```
┌─────────────────────────────────────────────────────────────┐
│                    RAG PIPELINE                              │
│                                                              │
│  ┌─────────────────────────────────────────┐                │
│  │  📚 BILIMLAR BAZASI (Knowledge Base)     │                │
│  │                                          │                │
│  │  ┌──────────────┐                        │                │
│  │  │ 📄 Tibbiy     │  Klinik protokollar,  │                │
│  │  │   Protokollar │  davolash sxemalari    │                │
│  │  └──────┬───────┘                        │                │
│  │         ▼                                │                │
│  │  ┌──────────────┐                        │                │
│  │  │ 🔤 Text       │  Matnlarni 512        │                │
│  │  │   Chunking   │  tokenli bo'laklarga   │                │
│  │  └──────┬───────┘  ajratish              │                │
│  │         ▼                                │                │
│  │  ┌──────────────┐                        │                │
│  │  │ 🧮 Embedding  │  OpenAI Ada-002 yoki  │                │
│  │  │   Model      │  all-MiniLM-L6-v2     │                │
│  │  └──────┬───────┘                        │                │
│  │         ▼                                │                │
│  │  ┌──────────────┐                        │                │
│  │  │ 💾 Vector     │  ChromaDB /           │                │
│  │  │   Store      │  Weaviate             │                │
│  │  └──────────────┘                        │                │
│  └─────────────────────────────────────────┘                │
│                                                              │
│  QIDIRUV JARAYONI:                                           │
│                                                              │
│  ┌──────────┐   ┌───────────┐   ┌─────────────┐            │
│  │  Bemor   │──▶│ Embedding │──▶│ Similarity  │            │
│  │  so'rovi │   │  (query)  │   │  Search     │            │
│  └──────────┘   └───────────┘   └──────┬──────┘            │
│                                        │                     │
│                                        ▼                     │
│                                 ┌─────────────┐             │
│                                 │  Top-K      │             │
│                                 │  Results    │             │
│                                 │  (k=5)      │             │
│                                 └──────┬──────┘             │
│                                        │                     │
│                                        ▼                     │
│                              ┌──────────────────┐           │
│                              │  LLM + Context   │           │
│                              │  → Final Answer  │           │
│                              └──────────────────┘           │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### 1.3.2. Bilimlar Bazasiga Yuklanadigan Ma'lumotlar

| # | Ma'lumot turi | Manbaa | Yangilanish |
|---|---------------|--------|-------------|
| 1 | Klinik protokollar | Sog'liqni saqlash vazirligi | Yiliga 1 marta |
| 2 | MKB-10 klassifikatsiya | WHO | Versiya yangilanganda |
| 3 | Dorilar ro'yxati | Farmatsevtika bazasi | Oyiga 1 marta |
| 4 | Klinika shifokorlari profili | CRM Ma'lumotlari | Real-vaqt |
| 5 | Xizmatlar va narxlar | Admin panel | Haftalik |
| 6 | Bemorlar fikrlari | Reviews moduli | Real-vaqt |
| 7 | Yo'nalishlar tavsifi | Marketing bo'limi | Oyiga 1 marta |

---

## 1.4. LLM System Prompt (AI Shaxsi)

### 1.4.1. System Prompt Shabloni

```
Sen — professional tibbiy yo'naltiruvchi assistantsan. Sening vazifang:

1. Bemorning simptomlarini diqqat bilan tahlil qilish.
2. Klinikaning 37 ta yo'nalishi ichidan eng to'g'ri bo'limni tavsiya qilish.
3. Hech qachon TASHXIS QO'YMASLIK — faqat qaysi SHIFOKORGA borishni maslahat berish.
4. Har doim javobingni quyidagi JSON formatda berish:

{
  "primary_department": {
    "name": "Nevrologiya",
    "mkb10_codes": ["R51", "H53.1"],
    "confidence": 0.87,
    "reason": "Bosh og'rig'i va ko'z qorong'ilashi nevrolog tekshiruvini talab qiladi."
  },
  "secondary_departments": [
    {
      "name": "Terapevt",
      "confidence": 0.45,
      "reason": "Umumiy tekshiruv uchun terapevt ham ko'rishi mumkin."
    },
    {
      "name": "Oftalmolog", 
      "confidence": 0.30,
      "reason": "Ko'z bilan bog'liq simptomlar sababli."
    }
  ],
  "urgency_level": "MEDIUM",
  "additional_questions": [
    "Bosh og'rig'i qachondan beri?",
    "Og'riq qaysi qismda — peshana, ensa yoki yon tomonlarda?"
  ],
  "disclaimer": "Bu AI tavsiyasi — tashxis emas. Mutaxassis ko'rigiga tashrif buyuring."
}

MUHIM QOIDALAR:
- Hech qachon "Sizda X kasallik bor" dema — faqat "Y shifokorga ko'rining" de.
- Shoshilinch holatlarda (ko'krak og'rig'i, hushdan ketish) darhol 103 ga qo'ng'iroq qilishni maslahat ber.
- Javoblarni O'ZBEK tilida, sodda va tushunarli qilib ber.
- Har doim 1 ta primary va 1-2 ta secondary yo'nalish ko'rsat.
```

### 1.4.2. Urgency (Shoshilinchlik) Darajalari

```
┌───────────────────────────────────────────────────────┐
│  SHOSHILINCHLIK DARAJALARI                            │
│                                                        │
│  🔴 CRITICAL (Juda shoshilinch):                      │
│      → Ko'krak og'rig'i                                │
│      → Hushdan ketish                                  │
│      → Nafas olish to'xtashi                           │
│      → Qon ketishi                                     │
│      HARAKAT: "103 ga zudlik bilan qo'ng'iroq qiling!" │
│                                                        │
│  🟠 HIGH (Yuqori):                                    │
│      → Kuchli bosh og'rig'i + qusish                   │
│      → Yuqori harorat (39°+)                           │
│      → Og'ir jarohatlari                               │
│      HARAKAT: "Bugun tezkor tibbiy yordam oling"       │
│                                                        │
│  🟡 MEDIUM (O'rtacha):                                │
│      → Bir necha kundan beri davom etayotgan og'riq    │
│      → Yengil shikoyatlar                              │
│      HARAKAT: "1-2 kun ichida shifokorga ko'rining"    │
│                                                        │
│  🟢 LOW (Past):                                       │
│      → Profilaktik tekshiruv                           │
│      → Umumiy savol                                    │
│      HARAKAT: "Qulay vaqtda shifokorga yoziling"       │
│                                                        │
└───────────────────────────────────────────────────────┘
```

---

## 1.5. Chat UI/UX

### 1.5.1. Chat Interfeysi

```
┌──────────────────────────────────────────────────────────┐
│  🤖 AI Tibbiy Yo'naltiruvchi                    [✕]     │
├──────────────────────────────────────────────────────────┤
│                                                          │
│  🤖 Salom! Men sizga qaysi shifokorga borishingiz       │
│     kerakligini aniqlashga yordam beraman.               │
│     Simptomlaringizni yozing.                            │
│                                                          │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─           │
│                                                          │
│                    👤 Boshim qattiq og'riyapti va        │
│                       ko'nglim ayniyapti                 │
│                                                          │
│  🤖 Tushundim. Bir nechta aniqlashtiruvchi savol:       │
│                                                          │
│     1️⃣ Bosh og'rig'i qachondan beri?                   │
│     2️⃣ Harorat bormi?                                   │
│     3️⃣ Avval ham shunday bo'lganmi?                     │
│                                                          │
│  ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─ ─           │
│                                                          │
│                    👤 2 kundan beri, harorat yo'q,       │
│                       birinchi marta                      │
│                                                          │
│  🤖 Tahlilim asosida sizga quyidagi mutaxassislarga       │
│     murojaat qilishni tavsiya qilaman:                   │
│                                                          │
│  ┌─────────────────────────────────────────┐             │
│  │ 🏆 TAVSIYA #1: NEVROLOGIYA      87%   │             │
│  │ Bosh og'rig'i va ko'ngil aynishi nevrolog │           │
│  │ tekshiruvini talab qilishi mumkin.        │           │
│  │                                           │           │
│  │ 👨‍⚕️ Eng yaxshi shifokorlar:                │           │
│  │ ├── Dr. Aliyev Xasan — ⭐4.9 — 15 yil    │           │
│  │ ├── Dr. Karimova Nila — ⭐4.8 — 12 yil   │           │
│  │ └── Dr. Saidov Bobur — ⭐4.7 — 8 yil     │           │
│  │                                           │           │
│  │ [📅 Navbatga Yozilish]                    │           │
│  └─────────────────────────────────────────┘             │
│                                                          │
│  ┌─────────────────────────────────────────┐             │
│  │ 2️⃣ Muqobil: TERAPEVT            45%    │             │
│  │ Umumiy tekshiruv uchun terapevtga        │             │
│  │ ham murojaat qilishingiz mumkin.          │             │
│  │ [📅 Navbatga Yozilish]                   │             │
│  └─────────────────────────────────────────┘             │
│                                                          │
│  ⚠️ Bu AI tavsiyasi — tashxis emas.                     │
│     Aniq tashxis faqat shifokor tomonidan qo'yiladi.     │
│                                                          │
├──────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────┐ [📎] [🎤]  │
│  │  Simptomlaringizni yozing...            │    [➤]     │
│  └─────────────────────────────────────────┘             │
│                                                          │
│  Tezkor: [🤒 Harorat] [🤕 Bosh] [🤢 Qusish] [💔 Yurak] │
└──────────────────────────────────────────────────────────┘
```

### 1.5.2. Tezkor Simptom Tugmalari

Inputdan pastda foydalanuvchi bir tugma bosish bilan tez tanlash qilishi uchun:

| Tugma | Simptomlar guruhi |
|-------|-------------------|
| 🤒 Harorat | Isitma, sovuq terlash |
| 🤕 Bosh og'riq | Migren, bosh aylanishi |
| 🤢 Hazm | Qusish, ich ketish, qorin og'rig'i |
| 💔 Yurak | Ko'krak og'rig'i, yurak urishi |
| 🦴 Suyak/Bo'g'in | Bel, tizza, qo'l og'rig'i |
| 👁️ Ko'z | Ko'rish pasayishi, qizarish |
| 🫁 Nafas | Yo'tal, nafas qisilishi |
| 🧴 Teri | Toshma, qichishish |

---

## 1.6. Intellektual Filtrlash va Ranking

### 1.6.1. Shifokorlarni Saralash Algoritmi

```
RANKING SCORE = W1 × (Reyting) 
              + W2 × (Tajriba_yillari / max_tajriba)
              + W3 × (Bo'sh_vaqt_yaqinligi)
              + W4 × (Narx_mosligi)
              + W5 × (Qayta_kelish_koeffitsienti)

Bu yerda:
  W1 = 0.30  (Reyting vazni)
  W2 = 0.20  (Tajriba vazni)
  W3 = 0.25  (Yaqin bo'sh vaqt vazni — eng tez qabul)
  W4 = 0.10  (Narx vazni — arzonroq = yuqoriroq ball)
  W5 = 0.15  (Qayta kelish — bemorlar takror kelishi)
```

### 1.6.2. Filtr Turlari

| Filtr | Izoh | Tezkor tugma matni |
|-------|------|-------------------|
| **Eng yuqori reyting** | Reytingi 4.5+ bo'lgan shifokorlar | "⭐ Eng yaxshi" |
| **Eng tajribali** | 10+ yil tajribaga ega | "👨‍⚕️ Eng tajribali" |
| **Eng arzon** | Eng past narxdagi xizmatlar | "💰 Eng arzon" |
| **Hozir bo'sh** | Bugun yoki ertaga bo'sh slotlari bor | "⚡ Hozir bo'sh" |
| **Onlayn konsultatsiya** | Masofaviy qabul qila oladigan | "💬 Onlayn" |
| **Jins bo'yicha** | Ayol/Erkak shifokor | "👩/👨" |

---

## 1.7. AI Xavfsizligi va Cheklovlar

### 1.7.1. Xavfsizlik Qoidalari

| Qoida | Implementatsiya |
|-------|-----------------|
| **PII Anonymization** | Bemor ismi, telefon raqami AI ga yuborilmaydi |
| **Rate Limiting** | Har bir foydalanuvchi kuniga max 50 ta so'rov |
| **Prompt Injection Protection** | Input sanitization + guardrails |
| **Medical Disclaimer** | Har bir javobda "Bu tashxis emas" ogohlantirishi |
| **No Drug Prescription** | AI hech qanday dori yozmaydi |
| **Audit Trail** | Barcha AI so'rovlar loglanadi (faqat anonimizatsiya qilingan) |

### 1.7.2. AI Monitoring va Analytics

```
┌────────────────────────────────────────────────┐
│  📊 AI ANALYTICS DASHBOARD (Admin uchun)       │
│                                                 │
│  Bugungi so'rovlar:        234                  │
│  O'rtacha javob vaqti:     2.3 sek             │
│  Muvaffaqiyatli yo'naltirish: 89%              │
│                                                 │
│  Top simptomlar (bu hafta):                     │
│  1. Bosh og'rig'i ─────── 45 ta (19.2%)        │
│  2. Harorat ───────────── 38 ta (16.2%)        │
│  3. Qorin og'rig'i ───── 29 ta (12.4%)        │
│  4. Yo'tal ────────────── 25 ta (10.7%)        │
│  5. Bel og'rig'i ──────── 18 ta (7.7%)         │
│                                                 │
│  Yo'naltirish bo'yicha:                         │
│  1. Terapevt ──────────── 28%                   │
│  2. Nevrologiya ────────── 18%                  │
│  3. LOR ──────────────── 14%                    │
│  4. Gastroenterologiya ── 12%                   │
│                                                 │
│  ⚠️ Trend signallari:                          │
│  "Allergiya" so'rovlari 40% oshdi (bahorda)     │
│  → Allergolog jadvalini kengaytirish tavsiya    │
│    etiladi.                                      │
│                                                 │
└────────────────────────────────────────────────┘
```

---

## 1.8. Texnik Implementatsiya Detallari

### 1.8.1. API Endpointlar

```
POST /api/v1/ai/symptom-check
  Body: {
    "message": "Boshim og'riyapti",
    "session_id": "uuid",
    "conversation_history": [...],
    "filters": {
      "preferred_gender": "any",
      "budget_range": "any",
      "time_preference": "nearest"
    }
  }
  
  Response: {
    "primary_department": {...},
    "secondary_departments": [...],
    "recommended_doctors": [...],
    "urgency_level": "MEDIUM",
    "follow_up_questions": [...],
    "session_id": "uuid"
  }

GET /api/v1/ai/quick-symptoms
  → Tezkor simptom tugmalari ro'yxatini qaytaradi

GET /api/v1/ai/analytics/trends
  → Admin uchun AI qidiruv trendlari
```

### 1.8.2. Ishlash Tezligi Talablari

| Metrika | Maqsad |
|---------|--------|
| Birinchi javob (First Token) | < 500ms |
| To'liq javob (Complete Response) | < 3 soniya |
| Vector Search | < 200ms |
| Bir vaqtda foydalanuvchilar | 100+ concurrent |

---

*Keyingi bo'lim: [03_SMART_BOOKING_VA_NAVBAT.md](./03_SMART_BOOKING_VA_NAVBAT.md)*
