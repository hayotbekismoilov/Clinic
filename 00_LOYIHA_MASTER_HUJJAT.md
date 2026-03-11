# 🏥 Klinika Boshqarish Ekotizimi — Med-Core Professional

## Texnik Topshiriq (TZ) — Master Hujjat v2.0

> **Loyiha nomi:** Med-Core Professional  
> **Versiya:** 2.0  
> **Yaratilgan sana:** 2026-03-09  
> **Mualliflar:** Development Team  
> **Holati:** Draft → Review → ✅ Approved  

---

## 📑 Mundarija

| # | Bo'lim | Fayl |
|---|--------|------|
| 00 | Loyiha Umumiy Tavsifi | **Shu hujjat** |
| 01 | Bosh Sahifa va Brending | [01_BOSH_SAHIFA_VA_BRENDING.md](./01_BOSH_SAHIFA_VA_BRENDING.md) |
| 02 | AI-Simptom Diagnostika | [02_AI_SIMPTOM_DIAGNOSTIKA.md](./02_AI_SIMPTOM_DIAGNOSTIKA.md) |
| 03 | Smart Booking va Navbat | [03_SMART_BOOKING_VA_NAVBAT.md](./03_SMART_BOOKING_VA_NAVBAT.md) |
| 04 | Bemor Shaxsiy Kabineti | [04_BEMOR_SHAXSIY_KABINETI.md](./04_BEMOR_SHAXSIY_KABINETI.md) |
| 05 | CRM Tibbiy Axborot Tizimi | [05_CRM_TIBBIY_AXBOROT_TIZIMI.md](./05_CRM_TIBBIY_AXBOROT_TIZIMI.md) |
| 06 | Moliya va Admin Panel | [06_MOLIYA_VA_ADMIN_PANEL.md](./06_MOLIYA_VA_ADMIN_PANEL.md) |
| 07 | Xavfsizlik va Arxitektura | [07_XAVFSIZLIK_VA_ARXITEKTURA.md](./07_XAVFSIZLIK_VA_ARXITEKTURA.md) |
| 08 | Ma'lumotlar Bazasi Sxemasi | [08_MALUMOTLAR_BAZASI.md](./08_MALUMOTLAR_BAZASI.md) |
| 09 | API Spetsifikatsiyasi | [09_API_SPETSIFIKATSIYA.md](./09_API_SPETSIFIKATSIYA.md) |
| 10 | Diagrammalar va Algoritmlar | [10_DIAGRAMMALAR_VA_ALGORITMLAR.md](./10_DIAGRAMMALAR_VA_ALGORITMLAR.md) |
| 11 | Roadmap va Sprint Rejasi | [11_ROADMAP_VA_SPRINT_REJASI.md](./11_ROADMAP_VA_SPRINT_REJASI.md) |
| 12 | Buyurtmachidan Kerakli Ma'lumotlar | [12_KERAKLI_MALUMOTLAR.md](./12_KERAKLI_MALUMOTLAR.md) |

---

## 1. Loyiha Umumiy Tavsifi

### 1.1. Maqsad
Ushbu loyiha zamonaviy klinikaning **barcha** biznes jarayonlarini yagona raqamli platformaga birlashtirish uchun mo'ljallangan. Tizim quyidagi muammolarni hal qiladi:

| Muammo | Yechim |
|--------|--------|
| Bemorlar qaysi shifokorga borishni bilmaydi | AI Simptom Diagnostika chatboti |
| Telefon orqali navbat olish qiyin | Onlayn Smart Booking tizimi |
| Tibbiy tarix qog'ozda saqlanadi | Elektron Tibbiy Karta (EHR) |
| Dori-darmonlar nazorat qilinmaydi | Inventar boshqaruv moduli |
| Moliyaviy hisobot qo'lda tayyorlanadi | Avtomatik analitika dashboardi |
| Bemorlar tahlil natijalarini kutadi | Real-vaqt laboratoriya integratsiyasi |

### 1.2. Ekotizim Arxitekturasi (Yuqori Daraja)

```
┌─────────────────────────────────────────────────────────────────┐
│                    KLINIKA EKOTIZIMI                             │
│                                                                  │
│  ┌──────────────┐    ┌──────────────┐    ┌──────────────┐       │
│  │  🌐 VEB-SAYT  │    │  📱 PWA APP   │    │  💬 TELEGRAM  │       │
│  │  (Next.js)   │    │  (React)     │    │  BOT          │       │
│  └──────┬───────┘    └──────┬───────┘    └──────┬───────┘       │
│         │                   │                   │                │
│         └───────────────────┼───────────────────┘                │
│                             │                                    │
│                    ┌────────▼────────┐                           │
│                    │   API GATEWAY   │                           │
│                    │   (NestJS)      │                           │
│                    └────────┬────────┘                           │
│                             │                                    │
│         ┌───────────────────┼───────────────────┐                │
│         │                   │                   │                │
│  ┌──────▼──────┐    ┌──────▼──────┐    ┌──────▼──────┐         │
│  │ 🤖 AI MODULE │    │ 📋 CRM/MIS  │    │ 💰 PAYMENT  │         │
│  │ (RAG+NLP)   │    │ (Med-Core)  │    │ (Click/Pay) │         │
│  └──────┬──────┘    └──────┬──────┘    └──────┬──────┘         │
│         │                   │                   │                │
│         └───────────────────┼───────────────────┘                │
│                             │                                    │
│                    ┌────────▼────────┐                           │
│                    │   DATABASE      │                           │
│                    │  PostgreSQL     │                           │
│                    │  Redis + Vector │                           │
│                    └─────────────────┘                           │
└─────────────────────────────────────────────────────────────────┘
```

### 1.3. Foydalanuvchi Rollari

| Rol | Tavsif | Kirish Huquqlari |
|-----|--------|------------------|
| **Bemor (Patient)** | Saytdan navbat oluvchi, tahlillarni ko'ruvchi | Shaxsiy kabinet, bron, tahlillar |
| **Shifokor (Doctor)** | Bemorlarni qabul qiluvchi, tashxis qo'yuvchi | EHR, navbatlar, retseptlar |
| **Hamshira (Nurse)** | Shifokorga yordamchi, vital belgilarni kirituvchi | Cheklangan EHR, navbatlar |
| **Registrator** | Bemorlarni ro'yxatga oluvchi | Bron tizimi, pasport ma'lumotlari |
| **Laborant** | Tahlil natijalarini kirituvchi | Lab moduli |
| **Kassir** | To'lovlarni qabul qiluvchi | Moliya moduli |
| **Omborchi** | Dori va uskunalarni boshqaruvchi | Inventar moduli |
| **Admin** | Tizimni to'liq boshqaruvchi | Barcha modullar |
| **Super Admin** | Tizim sozlamalarini o'zgartiruvchi | Tizim konfiguratsiyasi |

### 1.4. Texnologiyalar Stack'i

```
┌─────────────────────────────────────────────────────┐
│                  TECH STACK                          │
├─────────────────────────────────────────────────────┤
│                                                      │
│  FRONTEND                                            │
│  ├── Next.js 14+ (App Router, SSR/SSG)              │
│  ├── TypeScript (Strict mode)                        │
│  ├── Tailwind CSS + Shadcn/UI                       │
│  ├── Framer Motion (Animatsiyalar)                   │
│  ├── React Query (Server State)                      │
│  └── Zustand (Client State)                          │
│                                                      │
│  BACKEND                                             │
│  ├── NestJS (TypeScript, DI, Modular)               │
│  ├── Prisma ORM (Type-safe DB queries)              │
│  ├── Bull MQ (Job Queues - SMS, Email)              │
│  ├── Socket.IO (Real-time updates)                   │
│  └── Passport.js (Auth strategies)                   │
│                                                      │
│  AI / ML                                             │
│  ├── LangChain (RAG Pipeline)                        │
│  ├── OpenAI GPT-4o / Llama-3 (LLM)                 │
│  ├── ChromaDB / Weaviate (Vector Store)             │
│  └── spaCy (NLP - O'zbek tili uchun)               │
│                                                      │
│  DATABASE                                            │
│  ├── PostgreSQL 16 (Primary DB)                      │
│  ├── Redis 7 (Cache + Sessions + Pub/Sub)           │
│  ├── MinIO / S3 (File Storage - DICOM, PDF)         │
│  └── ClickHouse (Analytics, optional)               │
│                                                      │
│  INFRASTRUCTURE                                      │
│  ├── Docker + Docker Compose                         │
│  ├── Kubernetes (Production)                         │
│  ├── Nginx (Reverse Proxy + Load Balancer)          │
│  ├── GitHub Actions (CI/CD)                          │
│  └── Cloudflare (CDN + WAF + DDoS)                  │
│                                                      │
│  MONITORING                                          │
│  ├── Grafana + Prometheus (Metrics)                  │
│  ├── ELK Stack (Logging)                             │
│  └── Sentry (Error Tracking)                         │
│                                                      │
└─────────────────────────────────────────────────────┘
```

### 1.5. Glossariy (Lug'at)

| Atama | Ta'rifi |
|-------|---------|
| **EHR** | Electronic Health Record — Elektron tibbiy karta |
| **MIS** | Medical Information System — Tibbiy axborot tizimi |
| **CRM** | Customer Relationship Management — Mijozlar bilan aloqalarni boshqarish |
| **MKB-10** | Xalqaro kasalliklar klassifikatsiyasining 10-tahriri |
| **DICOM** | Digital Imaging and Communications in Medicine — Tibbiy tasvirlar standarti |
| **RAG** | Retrieval-Augmented Generation — Qidiruv orqali kuchaytirilgan generatsiya |
| **NLP** | Natural Language Processing — Tabiiy tilni qayta ishlash |
| **OTP** | One-Time Password — Bir martalik parol |
| **RBAC** | Role-Based Access Control — Rolga asoslangan kirish nazorati |
| **PWA** | Progressive Web App — Progressiv veb-ilova |
| **SSR** | Server-Side Rendering — Server tomonida render qilish |
| **JWT** | JSON Web Token — Autentifikatsiya tokeni |
| **HIPAA** | Health Insurance Portability and Accountability Act |
| **KPI** | Key Performance Indicator — Asosiy samaradorlik ko'rsatkichi |

---

## Batafsil Bo'limlar

> ⚠️ Har bir bo'lim alohida `.md` faylda batafsil tavsiflangan.  
> Yuqoridagi [Mundarija](#-mundarija) jadvalidagi havolalar orqali o'ting.

---

## Hujjatlar Ro'yxati

```
📁 Klinika TZ/
├── 📄 00_LOYIHA_MASTER_HUJJAT.md            ← Siz hozir shu yerdasiz
├── 📄 01_BOSH_SAHIFA_VA_BRENDING.md         ← UI/UX, 37 yo'nalish, 360° tur
├── 📄 02_AI_SIMPTOM_DIAGNOSTIKA.md          ← AI algoritmi, RAG pipeline
├── 📄 03_SMART_BOOKING_VA_NAVBAT.md         ← Navbat, slotlar, bildirishnomalar
├── 📄 04_BEMOR_SHAXSIY_KABINETI.md          ← Bemor portali, tahlillar
├── 📄 05_CRM_TIBBIY_AXBOROT_TIZIMI.md      ← EHR, shifokor dashboard, ombor
├── 📄 06_MOLIYA_VA_ADMIN_PANEL.md           ← To'lovlar, KPI, analitika
├── 📄 07_XAVFSIZLIK_VA_ARXITEKTURA.md       ← Shifrlash, RBAC, audit
├── 📄 08_MALUMOTLAR_BAZASI.md               ← Barcha jadvallar va munosabatlar
├── 📄 09_API_SPETSIFIKATSIYA.md             ← REST API spetsifikatsiyasi
├── 📄 10_DIAGRAMMALAR_VA_ALGORITMLAR.md     ← Mermaid diagrammalar to'plami
├── 📄 11_ROADMAP_VA_SPRINT_REJASI.md        ← Sprint rejasi va milestonelar
└── 📄 12_KERAKLI_MALUMOTLAR.md              ← Buyurtmachidan kerakli ma'lumotlar
```

---

*© 2026 Med-Core Professional. Barcha huquqlar himoyalangan.*
