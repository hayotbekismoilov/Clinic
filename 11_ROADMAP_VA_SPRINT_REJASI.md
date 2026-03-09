# 11. Roadmap va Sprint Rejasi

> **Loyiha davomiyligi:** 15-17 hafta (taxminan 4 oy)  
> **Metodologiya:** Agile Scrum (2 haftalik sprintlar)  
> **Mas'ul:** Project Manager + Tech Lead  

---

## 1.1. Umumiy Roadmap

```
LOYIHA TIMELINE:

═══════════════════════════════════════════════════════════════════════
                        2026 YIL
═══════════════════════════════════════════════════════════════════════
   MART          APREL         MAY           IYUN          IYUL
├──────────┼──────────────┼──────────────┼──────────────┼──────────┤
│          │              │              │              │          │
│ Sprint 1 │ Sprint 2-3   │ Sprint 4-5   │ Sprint 6-7   │Sprint 8 │
│          │              │              │              │          │
│ I BOSQICH│ II BOSQICH   │ III BOSQICH  │ IV BOSQICH   │V BOSQICH│
│ DIZAYN   │ CRM YADRO    │ AI + NOTIF.  │ TO'LOV+PWA   │ AUDIT   │
│          │              │              │              │          │
│ UI/UX    │ Database     │ RAG Pipeline │ Click/Payme  │ Testing │
│ Prototip │ Auth/EHR     │ NLP Engine   │ PWA optim.   │ Security│
│ Dizayn   │ Booking      │ Bildirishnoma│ Admin Panel  │ Launch! │
│ Tizimi   │ Lab Module   │ Bot          │ Analytics    │         │
│          │              │              │              │          │
│ 2-3 hafta│ 4-6 hafta    │ 3-4 hafta    │ 2-3 hafta    │ 2 hafta │
├──────────┼──────────────┼──────────────┼──────────────┼──────────┤
```

---

## 1.2. Sprint Tafsilotlari

### Sprint 1 (Hafta 1-2): Foundation & Design
```
┌──────────────────────────────────────────────────────────────┐
│  🎯 SPRINT 1: ASOS VA DIZAYN                                │
│  Muddat: Hafta 1-2                                           │
│                                                              │
│  📋 VAZIFALAR:                                               │
│                                                              │
│  ✅ P0 (Majburiy):                                          │
│  ├── Loyiha repozitoriyasini yaratish (monorepo)            │
│  ├── Next.js + NestJS scaffolding                            │
│  ├── Docker Compose (dev environment)                        │
│  ├── PostgreSQL + Redis + MinIO setup                        │
│  ├── Prisma ORM schema (barcha jadvallar)                   │
│  ├── Dizayn tizimi (Design Tokens yaratish)                 │
│  ├── Figma → Code: UI component library                     │
│  └── Landing page (Hero + 37 yo'nalish + Xarita)            │
│                                                              │
│  🟡 P1 (Muhim):                                             │
│  ├── CI/CD pipeline (GitHub Actions)                         │
│  ├── ESLint + Prettier + Husky setup                        │
│  └── Staging server deploy                                   │
│                                                              │
│  DELIVERABLE:                                                │
│  └── Sayt ochilganda landing page ko'rinadi                 │
│      37 ta yo'nalish, xarita, va dizayn tizimi tayyor       │
│                                                              │
│  TEAM:                                                       │
│  ├── Frontend Dev: Landing page + Components                │
│  ├── Backend Dev: Scaffolding + DB                          │
│  └── DevOps: Docker + CI/CD                                 │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

### Sprint 2 (Hafta 3-4): Auth & Patient Core
```
┌──────────────────────────────────────────────────────────────┐
│  🎯 SPRINT 2: AUTENTIFIKATSIYA VA BEMOR YADRO               │
│  Muddat: Hafta 3-4                                           │
│                                                              │
│  📋 VAZIFALAR:                                               │
│                                                              │
│  ✅ P0:                                                      │
│  ├── OTP autentifikatsiya (SMS + Telegram)                  │
│  ├── JWT token tizimi (access + refresh)                    │
│  ├── RBAC (rollar va huquqlar)                              │
│  ├── Patient CRUD (create, read, update)                    │
│  ├── Bemor profili sahifasi                                 │
│  ├── Shaxsiy kabinet (dashboard skeleton)                   │
│  └── EHR model (Elektron Tibbiy Karta yadrosi)             │
│                                                              │
│  🟡 P1:                                                     │
│  ├── Telegram bot bazasi (@MedCoreBot)                      │
│  ├── Session management (Redis)                              │
│  └── Audit log tizimi                                        │
│                                                              │
│  DELIVERABLE:                                                │
│  └── Bemor telefon orqali ro'yxatdan o'tadi va              │
│      shaxsiy kabinetga kiradi                                │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

### Sprint 3 (Hafta 5-6): Booking & Doctor Module
```
┌──────────────────────────────────────────────────────────────┐
│  🎯 SPRINT 3: BRON TIZIMI VA SHIFOKOR MODULI                │
│  Muddat: Hafta 5-6                                           │
│                                                              │
│  📋 VAZIFALAR:                                               │
│                                                              │
│  ✅ P0:                                                      │
│  ├── Shifokor profil sahifalari                              │
│  ├── Time Slot boshqaruvi (CRUD)                            │
│  ├── Smart Booking (slot tanlash + OTP)                     │
│  ├── Double-booking himoyasi (pessimistic lock)             │
│  ├── WebSocket real-time slot yangilanishi                   │
│  ├── Shifokor Dashboard (bugungi navbatlar)                 │
│  ├── Qabul boshlash / yakunlash workflow                    │
│  └── Registratura moduli                                     │
│                                                              │
│  🟡 P1:                                                     │
│  ├── Bekor qilish va ko'chirish                             │
│  ├── Virtual tur integratsiya (Pannellum.js)                │
│  └── Shifokor shablonlar tizimi                              │
│                                                              │
│  DELIVERABLE:                                                │
│  └── Bemor shifokorni tanlaydi, vaqt tanlaydi,             │
│      navbatga yoziladi. Shifokor o'z dashboardida           │
│      bugungi bemorlarni ko'radi.                             │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

### Sprint 4 (Hafta 7-8): Lab & EHR & Inventory
```
┌──────────────────────────────────────────────────────────────┐
│  🎯 SPRINT 4: LABORATORIYA, EHR VA OMBOR                    │
│  Muddat: Hafta 7-8                                           │
│                                                              │
│  📋 VAZIFALAR:                                               │
│                                                              │
│  ✅ P0:                                                      │
│  ├── MKB-10 autocomplete (tashxis qidirish)                 │
│  ├── Laboratoriya buyurtma → natija oqimi                   │
│  ├── Tahlil natijalari kiritish (laborant interfeysi)       │
│  ├── Bemor kabinetida tahlil ko'rsatish + grafik            │
│  ├── Elektron retsept yaratish                               │
│  ├── QR kod generatsiya (retsept uchun)                     │
│  ├── Ombor CRUD (dorilar, materiallar)                      │
│  ├── Avtomatik chiqarish algoritmi                           │
│  └── Muddati o'tayotgan dorilar ogohlantirishi              │
│                                                              │
│  🟡 P1:                                                     │
│  ├── DICOM viewer (tibbiy rasmlar)                          │
│  ├── PDF yuklab olish (tahlil natijalari)                   │
│  ├── Tahlillarni solishtirish (Compare view)                │
│  └── Vital belgilar kiritish va grafik                       │
│                                                              │
│  DELIVERABLE:                                                │
│  └── Shifokor tashxis qo'yadi, tahlil buyuradi,            │
│      retsept yozadi. Lab natija kiritadi.                    │
│      Bemor tahlillarini ko'radi. Ombor ishlaydi.            │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

### Sprint 5 (Hafta 9-10): AI Engine
```
┌──────────────────────────────────────────────────────────────┐
│  🎯 SPRINT 5: AI DIAGNOSTIKA ENGINE                         │
│  Muddat: Hafta 9-10                                          │
│                                                              │
│  📋 VAZIFALAR:                                               │
│                                                              │
│  ✅ P0:                                                      │
│  ├── ChromaDB setup va tilbiy protokollar yuklash           │
│  ├── O'zbekcha simptomlar lug'ati (2000+ termin)            │
│  ├── NLP preprocessing pipeline                              │
│  ├── RAG pipeline (query → vector search → LLM)            │
│  ├── AI Chat UI (real-time streaming)                       │
│  ├── Shoshilinchlik darajasi aniqlash                       │
│  ├── Shifokorlarni intellektual ranking                     │
│  └── Tezkor simptom tugmalari                                │
│                                                              │
│  🟡 P1:                                                     │
│  ├── AI tahlil sharhi (Lab results commentary)              │
│  ├── Prompt injection himoyasi                               │
│  ├── AI analytics dashboard (admin uchun)                   │
│  └── Rate limiting va logging                                │
│                                                              │
│  DELIVERABLE:                                                │
│  └── Bemor simptomlarni yozadi, AI shifokor tavsiya         │
│      qiladi, to'g'ridan-to'g'ri bron qilish mumkin.        │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

### Sprint 6 (Hafta 11-12): Notifications & Telegram Bot
```
┌──────────────────────────────────────────────────────────────┐
│  🎯 SPRINT 6: BILDIRISHNOMALAR VA TELEGRAM BOT              │
│  Muddat: Hafta 11-12                                         │
│                                                              │
│  📋 VAZIFALAR:                                               │
│                                                              │
│  ✅ P0:                                                      │
│  ├── BullMQ notification queue                               │
│  ├── SMS Gateway integratsiya (Eskiz.uz)                    │
│  ├── Eslatmalar: 3-kun, 24-soat, 2-soat                    │
│  ├── Emergency notifications                                 │
│  ├── Telegram Bot: OTP, eslatmalar, bron                    │
│  ├── Telegram inline buttons                                 │
│  ├── Dori qabul eslatma tizimi                              │
│  └── Qayta chaqirish (Retention) algoritmi                   │
│                                                              │
│  🟡 P1:                                                     │
│  ├── Web Push notifications                                  │
│  ├── Email notifications (ixtiyoriy)                         │
│  ├── Post-visit baholash so'rovi                            │
│  └── Bildirishnomalar markazi (in-app)                      │
│                                                              │
│  DELIVERABLE:                                                │
│  └── Barcha eslatmalar ishlaydi. Telegram bot orqali        │
│      navbat olish va boshqarish mumkin.                      │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

### Sprint 7 (Hafta 13-14): Payments & Admin
```
┌──────────────────────────────────────────────────────────────┐
│  🎯 SPRINT 7: TO'LOV INTEGRATSIYA VA ADMIN PANEL            │
│  Muddat: Hafta 13-14                                         │
│                                                              │
│  📋 VAZIFALAR:                                               │
│                                                              │
│  ✅ P0:                                                      │
│  ├── Click API integratsiya (prepare + complete)            │
│  ├── Payme API integratsiya (JSONRPC)                       │
│  ├── Zaklad (avans) to'lov tizimi                           │
│  ├── Refund (qaytarish) tizimi                              │
│  ├── Admin Dashboard (umumiy ko'rsatkichlar)                │
│  ├── Shifokorlar KPI hisoboti                               │
│  ├── Moliya hisoboti (Click/Payme/Naqd)                     │
│  ├── Yo'nalishlar statistikasi                               │
│  └── AI qidiruv trendlari dashboardi                        │
│                                                              │
│  🟡 P1:                                                     │
│  ├── Excel/PDF eksport                                       │
│  ├── PWA optimizatsiya (manifest, service worker)           │
│  ├── Offline mode (kesh strategiya)                          │
│  └── Mobile responsiveness final polish                      │
│                                                              │
│  DELIVERABLE:                                                │
│  └── Click/Payme to'lov ishlaydi. Admin barcha              │
│      statistikani ko'radi. PWA sifatida ishlaydi.           │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

### Sprint 8 (Hafta 15-17): Testing & Launch
```
┌──────────────────────────────────────────────────────────────┐
│  🎯 SPRINT 8: TESTLASH, XAVFSIZLIK VA ISHGA TUSHIRISH      │
│  Muddat: Hafta 15-17                                         │
│                                                              │
│  📋 VAZIFALAR:                                               │
│                                                              │
│  ✅ P0:                                                      │
│  ├── End-to-End testlar (Playwright)                        │
│  ├── Load testing (k6 / Artillery)                          │
│  ├── Security audit:                                         │
│  │   ├── SQL injection                                       │
│  │   ├── XSS                                                │
│  │   ├── CSRF                                                │
│  │   ├── Rate limiting                                       │
│  │   └── Data encryption verification                       │
│  ├── Performance optimization                                │
│  │   ├── Database query optimization                         │
│  │   ├── Image optimization (WebP)                          │
│  │   ├── Code splitting va lazy loading                     │
│  │   └── CDN konfiguratsiya                                 │
│  ├── Backup va disaster recovery test                       │
│  ├── Production deployment                                   │
│  ├── DNS va SSL setup                                        │
│  └── Monitoring setup (Grafana + Sentry)                    │
│                                                              │
│  🟡 P1:                                                     │
│  ├── Documentation (API docs, user guide)                   │
│  ├── Staff training (xodimlar treningi)                     │
│  └── Bug fixes va polishing                                  │
│                                                              │
│  DELIVERABLE:                                                │
│  └── 🚀 TIZIM PRODUCTION DA ISHGA TUSHDI!                   │
│      Barcha modullar ishlaydi, xavfsizlik tekshirildi,      │
│      monitoring o'rnatildi, xodimlar o'qitildi.             │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

## 1.3. Jamoa Tarkibi (Tavsiya)

```
MINIMAL JAMOA (6 kishi):

┌──────────────────────────────────────────────────────────────┐
│                                                              │
│  👨‍💼 Project Manager / Product Owner  ─── 1 kishi            │
│     └── Sprint rejalashtirish, prioritetlar, demo           │
│                                                              │
│  👨‍💻 Frontend Developer (Senior)  ─── 1 kishi               │
│     └── Next.js, UI components, responsive                  │
│                                                              │
│  👨‍💻 Backend Developer (Senior)  ─── 1 kishi                │
│     └── NestJS, Prisma, API, WebSocket                      │
│                                                              │
│  👨‍💻 Full-Stack Developer (Middle)  ─── 1 kishi             │
│     └── Admin panel, CRM, integrations                      │
│                                                              │
│  🤖 AI/ML Engineer  ─── 1 kishi                             │
│     └── RAG pipeline, NLP, LLM fine-tuning                  │
│                                                              │
│  🔧 DevOps Engineer (Part-time)  ─── 0.5 kishi             │
│     └── Docker, K8s, CI/CD, Monitoring                      │
│                                                              │
│  🎨 UI/UX Designer (Part-time)  ─── 0.5 kishi              │
│     └── Figma, dizayn tizimi, UX testing                    │
│                                                              │
│  🧪 QA Engineer (Part-time)  ─── 0.5 kishi                 │
│     └── Manual + automated testing                          │
│                                                              │
│  JAMI: ~6 full-time ekvivalent                              │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

## 1.4. Risklarni Boshqarish

```
RISK REGISTRY:

┌────┬──────────────────────────┬────────┬────────┬─────────────────────┐
│ #  │ Risk                     │Ehtimol │Ta'sir  │ Yechim              │
├────┼──────────────────────────┼────────┼────────┼─────────────────────┤
│ R1 │ AI aniqlik darajasi past │ O'rta  │ Yuqori │ A/B testing,        │
│    │                          │        │        │ fallback to manual  │
├────┼──────────────────────────┼────────┼────────┼─────────────────────┤
│ R2 │ O'zbek NLP modeli yo'q   │ Yuqori │ O'rta  │ Custom dictionary + │
│    │                          │        │        │ GPT-4o multilingual │
├────┼──────────────────────────┼────────┼────────┼─────────────────────┤
│ R3 │ Click/Payme API o'zgarishi│ Past  │ Yuqori │ Adapter pattern,    │
│    │                          │        │        │ versioned API calls │
├────┼──────────────────────────┼────────┼────────┼─────────────────────┤
│ R4 │ Shifokorlar tizimni      │ O'rta  │ Yuqori │ UX training,        │
│    │ ishlatmaydi              │        │        │ onboarding wizard   │
├────┼──────────────────────────┼────────┼────────┼─────────────────────┤
│ R5 │ Ma'lumotlar bazasi       │ Past   │ Kritik │ Daily backups,      │
│    │ yo'qolishi               │        │        │ 3-2-1 strategy      │
├────┼──────────────────────────┼────────┼────────┼─────────────────────┤
│ R6 │ DDoS hujumi              │ Past   │ Yuqori │ Cloudflare WAF,     │
│    │                          │        │        │ Rate limiting       │
├────┼──────────────────────────┼────────┼────────┼─────────────────────┤
│ R7 │ Kechikish (deadline)     │ O'rta  │ O'rta  │ MVP first approach, │
│    │                          │        │        │ P0/P1 prioritization│
└────┴──────────────────────────┴────────┴────────┴─────────────────────┘
```

---

## 1.5. Post-Launch Rejasi

```
ISHGA TUSHGANDAN KEYINGI BOSQICHLAR:

OYLIK MONITORING:
├── 1-oy:  Bug fixes, real user feedback, performance tuning
├── 2-oy:  A/B testing (AI aniqlik darajasi), UX improvements
├── 3-oy:  Mobile app (React Native) — agar kerak bo'lsa
├── 6-oy:  Telemedicina (video qabul) integratsiyasi
├── 9-oy:  Sug'urta kompaniyalari integratsiyasi
└── 12-oy: Multi-clinic support (tarmoq klinikalari uchun)

METRIKALR (KPI):
├── MAU (Monthly Active Users):     maqsad 5000+ (6 oyda)
├── Online booking conversion:       maqsad 40%+
├── AI recommendation accuracy:      maqsad 85%+
├── Patient satisfaction (NPS):      maqsad 70+
├── No-show rate:                    maqsad <10%
├── Average booking time:             maqsad <2 min
└── System uptime:                   maqsad 99.9%
```

---

## 1.6. Budget Taxmin (Oylik Xarajatlar)

```
INFRASTUKTURA XARAJATLARI (oylik):

┌──────────────────────────┬────────────────┐
│ Xizmat                   │ Taxminiy narx   │
├──────────────────────────┼────────────────┤
│ Cloud Server (2 vCPU,    │                │
│ 4GB RAM, 80GB SSD)       │ $30-50/oy      │
│ PostgreSQL managed       │ $20-40/oy      │
│ Redis managed            │ $15-25/oy      │
│ MinIO/S3 (100GB)         │ $5-10/oy       │
│ Cloudflare Pro           │ $20/oy         │
│ SMS Gateway (~2000 sms)  │ $20-30/oy      │
│ OpenAI API (~10K req)    │ $30-50/oy      │
│ Domain + SSL             │ $2/oy          │
│ Monitoring (Grafana Cloud)│ $0-15/oy      │
├──────────────────────────┼────────────────┤
│ JAMI                     │ ~$150-250/oy   │
│                          │ (~2-3 mln so'm)│
└──────────────────────────┴────────────────┘

DEVELOPMENT XARAJATLARI (bir martalik):
├── Jamoa (6 kishi × 4 oy):  Loyiha byudjetiga bog'liq
├── Dizayn (UI/UX):           Figma Pro ($15/oy × 4)
└── Litsenziyalar:            Asosan bepul (open-source stack)
```

---

*© 2026 Med-Core Professional. Barcha huquqlar himoyalangan.*  
*Ushbu hujjatlar to'plami Klinika Boshqarish Ekotizimining to'liq texnik tavsifi hisoblanadi.*
