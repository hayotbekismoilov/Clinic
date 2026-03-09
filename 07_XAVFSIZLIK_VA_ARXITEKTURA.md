# 07. Xavfsizlik va Texnik Arxitektura

> **Modul:** Security & Infrastructure  
> **Texnologiya:** Docker, Kubernetes, Cloudflare, PostgreSQL Encryption  
> **Mas'ul:** DevOps Engineer + Security Specialist  

---

## 1.1. Tizim Arxitekturasi (Batafsil)

### 1.1.1. Modular Monolith Arxitekturasi

```
MODULAR MONOLITH TUZILISHI:

┌──────────────────────────────────────────────────────────────┐
│                    NESTJS APPLICATION                         │
│                                                               │
│  ┌────────────────────────────────────────────────────────┐  │
│  │                    API GATEWAY                          │  │
│  │  ├── Rate Limiting (throttle)                          │  │
│  │  ├── JWT Validation                                    │  │
│  │  ├── Request Logging                                   │  │
│  │  └── CORS Policy                                       │  │
│  └────────────────────────┬───────────────────────────────┘  │
│                           │                                   │
│  ┌────────────────────────▼───────────────────────────────┐  │
│  │                    MODULES                              │  │
│  │                                                         │  │
│  │  ┌───────────┐ ┌───────────┐ ┌───────────┐            │  │
│  │  │  AUTH     │ │  PATIENT  │ │  DOCTOR   │            │  │
│  │  │  MODULE   │ │  MODULE   │ │  MODULE   │            │  │
│  │  │  --------│ │  --------│ │  --------│            │  │
│  │  │  OTP     │ │  EHR      │ │  Schedule │            │  │
│  │  │  JWT     │ │  Profile  │ │  Templates│            │  │
│  │  │  Sessions│ │  History  │ │  Visits   │            │  │
│  │  └───────────┘ └───────────┘ └───────────┘            │  │
│  │                                                         │  │
│  │  ┌───────────┐ ┌───────────┐ ┌───────────┐            │  │
│  │  │  BOOKING  │ │  AI       │ │  LAB      │            │  │
│  │  │  MODULE   │ │  MODULE   │ │  MODULE   │            │  │
│  │  │  --------│ │  --------│ │  --------│            │  │
│  │  │  Slots   │ │  RAG      │ │  Orders  │            │  │
│  │  │  Calendar│ │  NLP      │ │  Results │            │  │
│  │  │  Payment │ │  Analytics│ │  Reports │            │  │
│  │  └───────────┘ └───────────┘ └───────────┘            │  │
│  │                                                         │  │
│  │  ┌───────────┐ ┌───────────┐ ┌───────────┐            │  │
│  │  │  INVENTORY│ │  FINANCE  │ │  NOTIFICA.│            │  │
│  │  │  MODULE   │ │  MODULE   │ │  MODULE   │            │  │
│  │  │  --------│ │  --------│ │  --------│            │  │
│  │  │  Stock   │ │  Payments │ │  SMS     │            │  │
│  │  │  Expiry  │ │  Reports  │ │  Telegram│            │  │
│  │  │  Orders  │ │  KPI      │ │  Email   │            │  │
│  │  └───────────┘ └───────────┘ └───────────┘            │  │
│  │                                                         │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                               │
│  ┌────────────────────────────────────────────────────────┐  │
│  │                    SHARED LAYER                         │  │
│  │  ├── Prisma ORM (Database Access)                      │  │
│  │  ├── Redis Service (Cache/Pub-Sub)                     │  │
│  │  ├── MinIO Service (File Storage)                      │  │
│  │  ├── BullMQ Service (Job Queues)                       │  │
│  │  └── Logger Service (Winston/Pino)                     │  │
│  └────────────────────────────────────────────────────────┘  │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

### 1.1.2. Infrastructure Diagrammasi

```
DEPLOYMENT ARXITEKTURASI:

┌─────────────────────────────────────────────────────────────┐
│                    INTERNET                                  │
│                       │                                      │
│              ┌────────▼────────┐                            │
│              │  CLOUDFLARE     │                            │
│              │  ├── CDN        │                            │
│              │  ├── WAF        │                            │
│              │  ├── DDoS       │                            │
│              │  └── SSL/TLS    │                            │
│              └────────┬────────┘                            │
│                       │                                      │
│              ┌────────▼────────┐                            │
│              │  NGINX          │                            │
│              │  Load Balancer  │                            │
│              │  (Reverse Proxy)│                            │
│              └────────┬────────┘                            │
│                       │                                      │
│         ┌─────────────┼─────────────┐                       │
│         │             │             │                        │
│  ┌──────▼──────┐ ┌───▼────┐ ┌─────▼─────┐                 │
│  │  NEXT.JS    │ │ NESTJS │ │  NESTJS   │                 │
│  │  FRONTEND   │ │ API #1 │ │  API #2   │                 │
│  │  (SSR)      │ │        │ │ (replica) │                 │
│  │  Port: 3000 │ │Port:4000│ │Port:4001 │                 │
│  └─────────────┘ └────┬───┘ └─────┬─────┘                 │
│                        │           │                         │
│                   ┌────┴───────────┴────┐                   │
│                   │                      │                   │
│            ┌──────▼──────┐   ┌──────────▼──────┐           │
│            │ PostgreSQL  │   │  Redis Cluster   │           │
│            │ Primary     │   │  ├── Sessions    │           │
│            │ + Replica   │   │  ├── Cache       │           │
│            │             │   │  ├── BullMQ      │           │
│            │ Port: 5432  │   │  └── Pub/Sub     │           │
│            └─────────────┘   │  Port: 6379      │           │
│                              └──────────────────┘           │
│                                                              │
│         ┌─────────────┐    ┌─────────────┐                  │
│         │  MinIO       │    │  ChromaDB   │                  │
│         │  (S3-compat) │    │  (Vector DB)│                  │
│         │  Files/DICOM │    │  AI Search  │                  │
│         │  Port: 9000  │    │  Port: 8000 │                  │
│         └─────────────┘    └─────────────┘                  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 1.2. Ma'lumotlar Shifrlash

### 1.2.1. Encryption Strategiyasi

```
SHIFRLASH DARAJALARI:

1. TRANSPORT LAYER (Data in Transit):
   ├── TLS 1.3 (minimum)
   ├── HTTPS barcha endpointlarda majburiy
   ├── HSTS header: max-age=31536000; includeSubDomains
   └── Certificate: Let's Encrypt / Cloudflare Origin CA

2. DATABASE LAYER (Data at Rest):
   ├── PostgreSQL TDE (Transparent Data Encryption)
   ├── Alohida ustunlar uchun pgcrypto:
   │   ├── patient.phone → AES-256 shifrlangan
   │   ├── patient.address → AES-256 shifrlangan
   │   ├── medical_record.diagnosis → AES-256 shifrlangan
   │   └── medical_record.notes → AES-256 shifrlangan
   └── Encryption key: AWS KMS yoki HashiCorp Vault

3. FILE STORAGE (Data at Rest):
   ├── MinIO server-side encryption (SSE-S3)
   ├── Har bir fayl alohida kalit bilan shifrlangan
   └── DICOM fayllar uchun qo'shimcha layer

4. APPLICATION LAYER:
   ├── Parollar: bcrypt (saltRounds: 12)
   ├── OTP kodlar: SHA-256 hash
   ├── API keys: AES-256 + secure random
   └── Sensitive logs: masking (telefon: +998 90 ***-**-67)
```

### 1.2.2. JWT Xavfsizligi

```
JWT KONFIGURATSIYA:

{
  "algorithm": "RS256",           // RSA bilan imzolash (HS256 emas!)
  "accessTokenExpiry": "15m",     // 15 daqiqa
  "refreshTokenExpiry": "30d",    // 30 kun
  "issuer": "med-core-api",
  "audience": "med-core-client",
  
  "security": {
    "privateKey": "RSA 2048-bit (server da saqlanadi)",
    "publicKey": "Verifikatsiya uchun frontend ga berilishi mumkin",
    "tokenBlacklist": "Redis SET (logout qilganda)",
    "refreshRotation": true,       // Har safar yangi refresh token
    "fingerprintBinding": true     // Device fingerprint bilan bog'lash
  }
}

LOGOUT OQIMI:
1. Client: POST /api/v1/auth/logout
2. Server: Access token → Redis blacklist ga qo'shiladi
3. Server: Refresh token → DB dan o'chiriladi
4. Client: Local storage tozalanadi
```

---

## 1.3. RBAC (Role-Based Access Control)

### 1.3.1. Huquqlar Tizimi

```
PERMISSION TUZILISHI:

Har bir ruxsat 3 qismdan iborat:
  <resource>:<action>:<scope>

Misollar:
  patients:read:own        → Shifokor faqat o'z bemorlarini ko'radi
  patients:read:all        → Admin barcha bemorlarni ko'radi
  patients:write:own       → O'z bemorini tahrirlash
  bookings:create:any      → Har qanday bemor uchun bron yaratish
  finance:read:all         → Barcha moliyaviy ma'lumotlar
  settings:write:system    → Tizim sozlamalarini o'zgartirish

GUARD IMPLEMENTATSIYA (NestJS):
@UseGuards(JwtAuthGuard, RolesGuard)
@Roles('DOCTOR')
@Permissions('patients:read:own')
@Get('/patients/:id')
async getPatient(@Param('id') id: string, @CurrentUser() user: User) {
  // Faqat o'z bemori bo'lsa ruxsat beradi
  return this.patientService.findOne(id, user);
}
```

---

## 1.4. Audit Log (Tekshiruv Jurnali)

### 1.4.1. Audit Log Tuzilishi

```
AUDIT LOG MA'LUMOTLARI:

Har bir muhim harakat loglanadi:

{
  "id": "AUD-2026-030910-001234",
  "timestamp": "2026-03-09T10:30:45.123Z",
  "actor": {
    "id": "DOC-K-001",
    "role": "DOCTOR",
    "name": "Dr. Aliyev Xasan",
    "ip": "192.168.1.45",
    "user_agent": "Chrome/120, Windows 11",
    "session_id": "SES-abcdef123456"
  },
  "action": "READ",
  "resource": {
    "type": "PATIENT_RECORD",
    "id": "PAT-00001234",
    "name": "Ahmadov Sherzod"
  },
  "details": {
    "fields_accessed": ["diagnosis", "lab_results", "prescriptions"],
    "reason": "Scheduled appointment visit"
  },
  "result": "SUCCESS"
}

LOGLANADIGAN HARAKATLAR:
├── 👤 AUTH: Login, Logout, Failed login, Password change
├── 📋 PATIENT: View, Create, Update, Delete record
├── 💊 PRESCRIPTION: Create, Modify, Cancel
├── 🔬 LAB: Order, View results
├── 💰 FINANCE: Payment, Refund, Report view
├── ⚙️ SYSTEM: Settings change, User management
├── 📁 FILE: Upload, Download, Delete (DICOM, PDF)
└── 🤖 AI: Symptom query (anonymized)

SAQLASH MUDDATI: Minimum 5 yil (tibbiy qonunchilikka muvofiq)
SAQLASH JOYI: Alohida PostgreSQL jadvali (asosiy DB dan ajratilgan)
```

---

## 1.5. Backup va Disaster Recovery

### 1.5.1. Backup Strategiyasi

```
ZAXIRA NUSXA STRATEGIYASI:

┌──────────────────────────────────────────────────────────────┐
│  BACKUP SCHEDULE                                             │
│                                                               │
│  ┌───────────────────┬──────────────┬──────────────────────┐ │
│  │ Turi               │ Chastotasi   │ Saqlash muddati      │ │
│  ├───────────────────┼──────────────┼──────────────────────┤ │
│  │ Full DB backup     │ Har kuni     │ 30 kun               │ │
│  │                    │ (02:00)      │                      │ │
│  │ Incremental backup │ Har soatda   │ 7 kun                │ │
│  │ WAL Archiving      │ Real-vaqt    │ 7 kun                │ │
│  │ File storage       │ Har kuni     │ 90 kun               │ │
│  │ Config backup      │ Har o'zgarish│ Cheksiz              │ │
│  └───────────────────┴──────────────┴──────────────────────┘ │
│                                                               │
│  SAQLASH JOYLARI (3-2-1 qoidasi):                            │
│  3 ta nusxa:                                                  │
│  ├── 1. Primary server local disk                            │
│  ├── 2. Alohida server (boshqa data center)                  │
│  └── 3. Cloud storage (Google Cloud / AWS S3)                │
│                                                               │
│  2 ta turli muhit:                                            │
│  ├── Lokal server                                             │
│  └── Cloud                                                    │
│                                                               │
│  1 ta offsite:                                                │
│  └── Boshqa shahardagi/mamlakatdagi server                   │
│                                                               │
│  TIKLANISH VAQTI (RTO/RPO):                                  │
│  ├── RPO (Recovery Point Objective): < 1 soat               │
│  │   (1 soatlik ma'lumot yo'qotish maksimal)                 │
│  └── RTO (Recovery Time Objective): < 4 soat                │
│      (4 soat ichida tizim qayta ishga tushadi)               │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

### 1.5.2. Disaster Recovery Plan

```
FAVQULODDA TIKLANISH REJASI:

SENARIY 1: Server ishdan chiqishi
├── Nginx → avtomatik ravishda 2-server ga yo'naltiradi
├── PostgreSQL replica mastering ga o'tadi
└── Downtime: < 5 daqiqa (avtomatik failover)

SENARIY 2: Database corruption
├── Oxirgi WAL log dan point-in-time recovery
├── Incremental backup dan tiklanish
└── Downtime: 1-2 soat

SENARIY 3: Ransomware / Hacker hujumi
├── Tizimni tarmoqdan uzish
├── Offsite backup dan tozalangan serverga tiklanish
├── Security audit va patching
└── Downtime: 4-12 soat

SENARIY 4: Data center favqulodda holat
├── Cloud backup (AWS/GCP) dan boshqa data center ga deploy
└── Downtime: 4-8 soat
```

---

## 1.6. Monitoring va Alerting

### 1.6.1. Monitoring Stack

```
MONITORING TUZILISHI:

┌──────────────────────────────────────────────────────────────┐
│                    MONITORING STACK                           │
│                                                               │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐       │
│  │  PROMETHEUS  │   │  GRAFANA    │   │  SENTRY     │       │
│  │  (Metrics)   │   │  (Dashbords)│   │  (Errors)   │       │
│  └──────┬──────┘   └──────┬──────┘   └──────┬──────┘       │
│         │                  │                  │               │
│  Nima o'lchanadi:          │    Nima ko'rsatiladi:           │
│  ├── CPU/RAM/Disk          │    ├── Server holati            │
│  ├── Request per sec       │    ├── Error rate               │
│  ├── Response time         │    ├── DB queries/sec           │
│  ├── DB connections        │    ├── Active users             │
│  ├── Queue size            │    └── Business metrics         │
│  └── Cache hit rate        │                                 │
│                            │                                 │
│  ALERTING QOIDALARI:       │                                 │
│  ├── CPU > 80% 5 min davom → Telegram + SMS                 │
│  ├── Memory > 90%         → Telegram + SMS                  │
│  ├── Response time > 3s   → Telegram                        │
│  ├── Error rate > 5%      → Telegram + SMS + Call           │
│  ├── DB connections > 80% → Telegram                        │
│  ├── Disk > 85%           → Telegram + SMS                  │
│  └── Service down         → Telegram + SMS + Call           │
│                                                               │
└──────────────────────────────────────────────────────────────┘
```

---

## 1.7. CI/CD Pipeline

### 1.7.1. Deployment Oqimi

```
CI/CD PIPELINE (GitHub Actions):

┌────────┐   ┌────────┐   ┌────────┐   ┌────────┐   ┌────────┐
│  CODE  │──▶│  TEST  │──▶│ BUILD  │──▶│ STAGE  │──▶│  PROD  │
│  PUSH  │   │        │   │        │   │        │   │        │
└────────┘   └────────┘   └────────┘   └────────┘   └────────┘

1. CODE PUSH (Develop branch):
   └── Git push → GitHub webhook triggers

2. TEST (avtomatik):
   ├── ESLint + Prettier (code quality)
   ├── Unit tests (Jest, coverage > 80%)
   ├── Integration tests
   ├── E2E tests (Playwright)
   └── Security scan (npm audit, Snyk)

3. BUILD:
   ├── Docker image build (multi-stage)
   ├── Next.js build (SSR + static)
   ├── NestJS build (TypeScript → JavaScript)
   └── Image push → Container Registry

4. STAGING:
   ├── Deploy to staging server
   ├── Smoke tests
   ├── QA manual review
   └── Approval gate (manual)

5. PRODUCTION:
   ├── Blue-Green deployment
   ├── Health check
   ├── Rollback agar xatolik bo'lsa (avtomatik)
   └── Monitoring alert reset
```

---

*Keyingi bo'lim: [08_MALUMOTLAR_BAZASI.md](./08_MALUMOTLAR_BAZASI.md)*
