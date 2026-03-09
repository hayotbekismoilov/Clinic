# 10. Diagrammalar va Algoritmlar To'plami

> **Format:** Mermaid Diagrammalar (GitHub da avtomatik renderlanadi)  
> **Mas'ul:** System Architect  

---

## 1.1. Tizim Umumiy Arxitektura Diagrammasi

```mermaid
graph TB
    subgraph "Frontend Layer"
        WEB[🌐 Next.js Website]
        PWA[📱 PWA Mobile]
        TG[💬 Telegram Bot]
    end

    subgraph "API Gateway"
        NGINX[Nginx Load Balancer]
        API[NestJS API Server]
    end

    subgraph "Core Services"
        AUTH[🔐 Auth Module]
        BOOK[📅 Booking Module]
        PAT[👤 Patient Module]
        DOC[👨‍⚕️ Doctor Module]
        LAB[🔬 Lab Module]
        INV[📦 Inventory Module]
        FIN[💰 Finance Module]
        NOT[🔔 Notification Module]
    end

    subgraph "AI Layer"
        NLP[📝 NLP Processor]
        RAG[🔍 RAG Engine]
        LLM[🤖 LLM - GPT-4o]
        VDB[(ChromaDB Vector Store)]
    end

    subgraph "Data Layer"
        PG[(PostgreSQL)]
        REDIS[(Redis Cache)]
        MINIO[(MinIO Files)]
        BULL[⚡ BullMQ Jobs]
    end

    subgraph "External Services"
        SMS[📱 SMS Gateway]
        CLICK[💳 Click API]
        PAYME[💳 Payme API]
        MAPS[🗺️ Google Maps]
    end

    WEB --> NGINX
    PWA --> NGINX
    TG --> API
    NGINX --> API

    API --> AUTH
    API --> BOOK
    API --> PAT
    API --> DOC
    API --> LAB
    API --> INV
    API --> FIN
    API --> NOT

    BOOK --> NLP
    NLP --> RAG
    RAG --> VDB
    RAG --> LLM

    AUTH --> PG
    AUTH --> REDIS
    BOOK --> PG
    PAT --> PG
    DOC --> PG
    LAB --> PG
    INV --> PG
    FIN --> PG
    PAT --> MINIO
    NOT --> BULL
    BULL --> SMS
    BULL --> TG
    FIN --> CLICK
    FIN --> PAYME
```

---

## 1.2. Bemor Sayohati (Patient Journey) — Sequence Diagram

```mermaid
sequenceDiagram
    actor B as 👤 Bemor
    participant S as 🌐 Sayt
    participant AI as 🤖 AI Engine
    participant API as ⚙️ Backend
    participant DB as 💾 Database
    participant SMS as 📱 SMS/Telegram
    participant DOC as 👨‍⚕️ Shifokor
    participant LAB as 🔬 Laboratoriya

    Note over B,LAB: ═══ 1-BOSQICH: AI DIAGNOSTIKA ═══

    B->>S: Simptomlarni yozadi
    S->>AI: POST /ai/symptom-check
    AI->>AI: NLP Pre-processing
    AI->>AI: RAG Vector Search
    AI->>AI: LLM Inference
    AI-->>S: Tavsiya: Nevrologiya (87%)
    S-->>B: Shifokorlar ro'yxati ko'rsatiladi

    Note over B,LAB: ═══ 2-BOSQICH: BRON QILISH ═══

    B->>S: Shifokor va vaqt tanlaydi
    S->>API: POST /appointments
    API->>DB: SELECT FOR UPDATE (slot lock)
    DB-->>API: Slot AVAILABLE ✅
    API->>DB: UPDATE status = PENDING
    API->>SMS: OTP kod yuborish
    SMS-->>B: "Kod: 847291"
    B->>S: OTP kodni kiritadi
    S->>API: POST /auth/verify-otp
    API->>DB: UPDATE status = CONFIRMED
    API->>SMS: Tasdiqlash xabari
    SMS-->>B: "Navbat tasdiqlandi! #MED-2026-0415"

    Note over B,LAB: ═══ 3-BOSQICH: ESLATMALAR ═══

    API->>SMS: [T-3 kun] Eslatma
    SMS-->>B: "3 kundan so'ng qabul"
    API->>SMS: [T-24h] Tasdiqlash so'rovi
    SMS-->>B: "Ertaga 10:00. Kelasizmi?"
    B-->>SMS: "1 - Kelaman"

    Note over B,LAB: ═══ 4-BOSQICH: QABUL ═══

    B->>S: Klinikaga keladi
    S->>API: Registratura "Keldi" button
    API->>DB: Status = ARRIVED
    DOC->>API: "Qabul Boshlash"
    API->>DB: Bemor tarixini yuklash
    DB-->>DOC: EHR ma'lumotlari
    DOC->>API: Tashxis + Retsept kiritish
    API->>DB: Visit, Prescription saqlash
    API->>LAB: Tahlil buyurtma
    API->>SMS: Retsept yuborish
    SMS-->>B: "Retseptingiz: [PDF link]"
    DOC->>API: "Qabul Yakunlash"

    Note over B,LAB: ═══ 5-BOSQICH: TAHLIL NATIJALARI ═══

    LAB->>API: Natijalarni kiritish
    API->>DB: Lab results saqlash
    API->>AI: AI sharh generatsiya
    AI-->>API: Tushunarli tildagi sharh
    API->>SMS: "Tahlil natijangiz tayyor!"
    SMS-->>B: Link → Shaxsiy kabinet
    B->>S: Natijalarni ko'radi + AI sharh o'qiydi

    Note over B,LAB: ═══ 6-BOSQICH: FOLLOW-UP ═══

    API->>SMS: [+1 soat] "Shifokorni baholang ⭐"
    B-->>SMS: "⭐⭐⭐⭐⭐"
    API->>SMS: [+30 kun] "Kontrol qabul vaqti!"
    B->>S: Qayta yozilish (One-click)
```

---

## 1.3. Bron Qilish State Machine

```mermaid
stateDiagram-v2
    [*] --> AVAILABLE: Slot yaratildi

    AVAILABLE --> PENDING: Bemor slot tanladi
    PENDING --> AVAILABLE: OTP vaqti tugdi (15 min)
    PENDING --> CONFIRMED: OTP tasdiqlandi + To'lov
    PENDING --> AVAILABLE: Bemor bekor qildi

    CONFIRMED --> ARRIVED: Bemor klinikaga keldi
    CONFIRMED --> RESCHEDULED: Vaqt ko'chirildi
    CONFIRMED --> CANCELLED: 24h oldin bekor
    CONFIRMED --> NO_SHOW: Bemor kelmadi

    RESCHEDULED --> CONFIRMED: Yangi vaqt tasdiqlandi
    RESCHEDULED --> CANCELLED: Bekor qilindi

    ARRIVED --> IN_PROGRESS: Shifokor qabul boshladi
    IN_PROGRESS --> COMPLETED: Qabul yakunlandi

    CANCELLED --> AVAILABLE: Slot qayta ochildi
    NO_SHOW --> AVAILABLE: Slot qayta ochildi

    COMPLETED --> [*]

    note right of PENDING
        15 daqiqa ichida OTP
        tasdiqlanishi kerak
    end note

    note right of CONFIRMED
        3 kun, 24h, 2h oldin
        eslatma yuboriladi
    end note

    note right of NO_SHOW
        3+ no-show = 100% prepaid
        keyingi bron uchun
    end note
```

---

## 1.4. AI Diagnostika Oqimi

```mermaid
flowchart TD
    A[👤 Bemor simptom yozadi] --> B{Matn bo'shmi?}
    B -- Ha --> C[❌ Simptomlaringizni yozing]
    B -- Yo'q --> D[📝 NLP Pre-processing]
    
    D --> D1[Matnni tozalash]
    D1 --> D2[Tokenizatsiya]
    D2 --> D3[Simptom Entity Recognition]
    D3 --> D4[O'zbekcha → Tibbiy termin]
    
    D4 --> E{Shoshilinchmi?}
    E -- 🔴 CRITICAL --> F[🚨 103 ga qo'ng'iroq qiling!]
    E -- Yo'q --> G[🔍 RAG Vector Search]
    
    G --> G1[ChromaDB dan mos protokollar]
    G1 --> G2[Top-5 kontekst]
    G2 --> H[🤖 LLM Inference]
    
    H --> I[📊 Structured JSON Output]
    I --> J{Ishonch > 60%?}
    J -- Ha --> K[✅ Yo'nalish tavsiya]
    J -- Yo'q --> L[❓ Qo'shimcha savol berish]
    L --> A
    
    K --> M[👨‍⚕️ Top-3 shifokor ko'rsatish]
    M --> N[📅 Navbatga yozilish tugmasi]
    N --> O[Smart Booking oqimiga o'tish]
    
    style F fill:#ff4444,color:#fff
    style K fill:#44bb44,color:#fff
    style L fill:#ffaa00,color:#fff
```

---

## 1.5. To'lov Oqimi

```mermaid
flowchart TD
    A[Bemor to'lov usulini tanlaydi] --> B{Qaysi usul?}
    
    B -- Click --> C[Click invoice yaratish]
    C --> C1[redirect: my.click.uz]
    C1 --> C2{Bemor to'ladimi?}
    C2 -- Ha --> C3[Click callback: Prepare]
    C3 --> C4[Click callback: Complete]
    C4 --> G
    C2 -- Yo'q --> H[❌ To'lov bekor]
    
    B -- Payme --> D[Payme receipt yaratish]
    D --> D1[redirect: checkout.paycom.uz]
    D1 --> D2{Bemor to'ladimi?}
    D2 -- Ha --> D3[Payme: CreateTransaction]
    D3 --> D4[Payme: PerformTransaction]
    D4 --> G
    D2 -- Yo'q --> H
    
    B -- Naqd --> E[Status: CASH_PENDING]
    E --> E1[Klinikada kassaga to'lash]
    E1 --> G
    
    G[✅ To'lov tasdiqlandi] --> G1[Bron CONFIRMED]
    G1 --> G2[SMS/Telegram: Tasdiqlash]
    G1 --> G3[Moliya bazasiga yozish]
    
    H --> H1[Bron CANCELLED]
    H1 --> H2[Slot qayta AVAILABLE]
    
    style G fill:#44bb44,color:#fff
    style H fill:#ff4444,color:#fff
```

---

## 1.6. Laboratoriya Oqimi

```mermaid
flowchart LR
    A[👨‍⚕️ Shifokor tahlil buyuradi] --> B[📋 Buyurtma yaratildi]
    B --> C[🏷️ Shtrix-kod generatsiya]
    C --> D[👩 Hamshira qon oladi]
    D --> E[🔬 Laboratoriya tahlil qiladi]
    E --> F{Natija tayyor?}
    F -- Ha --> G[📊 Natija kiritiladi]
    G --> H[🤖 AI auto-validation]
    H --> I{Normadan og'ish bormi?}
    I -- Ha --> J[⚠️ Shifokorga ogohlantirish]
    I -- Yo'q --> K[✅ Normal]
    J --> L[📱 Bemorga bildirishnoma]
    K --> L
    L --> M[👤 Bemor shaxsiy kabinetda ko'radi]
    M --> N[📥 PDF yuklab olish]
```

---

## 1.7. Ombor Avtomatik Chiqarish

```mermaid
flowchart TD
    A[👨‍⚕️ Shifokor qabul yakunlaydi] --> B[Tashxis + Muolaja kiritildi]
    B --> C[📋 Muolaja-Material jadval tekshiruvi]
    C --> D{Material aniqlandi?}
    
    D -- Ha --> E[📦 Ombordan chiqarish]
    E --> F{Minimal miqdordan kammi?}
    
    F -- Ha --> G[🔴 Ogohlantirish: Kam qoldi!]
    G --> G1[📧 Omborchi + Admin ga xabar]
    G1 --> G2[📋 Avtomatik buyurtma taklifi]
    
    F -- Yo'q --> H[✅ Ombordan muvaffaqiyatli chiqarildi]
    
    D -- Yo'q --> I[⚠️ Qo'lda kiritish kerak]
    
    H --> J[📊 Sarflanish hisobotiga yozish]
    
    style G fill:#ff4444,color:#fff
    style H fill:#44bb44,color:#fff
```

---

## 1.8. Bildirishnomalar Oqimi

```mermaid
flowchart TD
    A[📅 Event sodir bo'ldi] --> B{Event turi?}
    
    B -- Bron tasdiqlandi --> C1[SMS: Tasdiq + kod]
    B -- 3 kun oldin --> C2[Telegram: Eslatma]
    B -- 24 soat oldin --> C3[SMS + TG: Tasdiqlash]
    B -- 2 soat oldin --> C4[Push: Manzil + yo'nalish]
    B -- Emergency --> C5[SMS + TG + Push: Bekor!]
    B -- Tahlil tayyor --> C6[SMS + TG: Natija tayyor]
    B -- Qabul yakunlandi --> C7[TG: Baholash so'rovi]
    B -- Dori eslatma --> C8[TG: Dori qabul vaqti]
    B -- Qayta chaqirish --> C9[TG: Tekshiruv vaqti]
    
    C1 --> D[⚡ BullMQ Job Queue]
    C2 --> D
    C3 --> D
    C4 --> D
    C5 --> D
    C6 --> D
    C7 --> D
    C8 --> D
    C9 --> D
    
    D --> E{Kanal?}
    E -- SMS --> F1[📱 Eskiz SMS API]
    E -- Telegram --> F2[💬 TG Bot API]
    E -- Email --> F3[📧 SMTP]
    E -- Push --> F4[🔔 Web Push API]
    
    F1 --> G[📊 Delivery status loglash]
    F2 --> G
    F3 --> G
    F4 --> G
```

---

## 1.9. RBAC Huquqlar Diagrammasi

```mermaid
graph TD
    SA[🔐 Super Admin] --> A[👑 Admin]
    A --> DOC[👨‍⚕️ Shifokor]
    A --> NRS[👩‍⚕️ Hamshira]
    A --> REG[📋 Registrator]
    A --> KAS[💰 Kassir]
    A --> LAB[🔬 Laborant]
    A --> OMB[📦 Omborchi]
    
    SA -.->|Tizim sozlamalari| SYS[⚙️ System Config]
    SA -.->|Audit loglar| AUD[📜 Audit Logs]
    
    A -.->|Barcha modullar| ALL[📊 Full Dashboard]
    A -.->|Foydalanuvchilar| USR[👥 User Management]
    
    DOC -.->|O'z bemorlari| EHR[📋 EHR]
    DOC -.->|Retseptlar| RX[💊 Prescriptions]
    DOC -.->|Tahlil buyurtma| LO[🔬 Lab Orders]
    
    NRS -.->|Vital belgilar| VIT[📊 Vitals]
    NRS -.->|Cheklangan EHR| LEHR[📋 Limited EHR]
    
    REG -.->|Navbatlar| APT[📅 Appointments]
    REG -.->|Pasport ma'lumot| PAS[🪪 Patient Info]
    
    KAS -.->|To'lovlar| PAY[💳 Payments]
    LAB -.->|Natijalar| RES[📊 Lab Results]
    OMB -.->|Inventar| INV[📦 Inventory]
```

---

## 1.10. Deployment Pipeline

```mermaid
flowchart LR
    A[👨‍💻 Code Push] --> B[🔍 Lint + Format]
    B --> C[🧪 Unit Tests]
    C --> D[🧪 Integration Tests]
    D --> E[🔒 Security Scan]
    E --> F{Barcha testlar ✅?}
    
    F -- Yo'q --> G[❌ Build Failed → Tuzatish]
    F -- Ha --> H[🐳 Docker Build]
    
    H --> I[📦 Registry Push]
    I --> J[🟡 Staging Deploy]
    J --> K[🧪 Smoke Tests]
    K --> L{QA Approved?}
    
    L -- Yo'q --> M[🔄 Fix → Retry]
    L -- Ha --> N[🟢 Production Deploy]
    
    N --> O[🏥 Health Check]
    O --> P{Sog'lommi?}
    P -- Ha --> Q[✅ DONE]
    P -- Yo'q --> R[🔴 Auto Rollback]
    
    style Q fill:#44bb44,color:#fff
    style G fill:#ff4444,color:#fff
    style R fill:#ff4444,color:#fff
```

---

*Keyingi bo'lim: [11_ROADMAP_VA_SPRINT_REJASI.md](./11_ROADMAP_VA_SPRINT_REJASI.md)*
