# 🏥 Med-Core Professional — Scrum Boshqaruv Tizimi

> **Scrum Maturity Level:** 5/5 · **Team:** 6 nafar · **Velocity:** 31 SP · **Sprint:** 2 hafta

---

## 📊 Sprint 1 — Joriy Holat

| Metrika | Qiymat | Izoh |
|---|---|---|
| **Sprint Progress** | 62% | 19 / 31 SP bajarildi |
| **Backlog hajmi** | 7 ta | User Story + Task |
| **Velocity (o'rtacha)** | 31 SP | Oxirgi 3 sprint bo'yicha |
| **Bug / Defect** | 0 | Staging muhitida |
| **Sprint qolgan vaqt** | 7 kun | Jami 14 kunlik sprint |

---

## 🎭 1. User Personas (Foydalanuvchi Personajlari)

### Anora opa — Bemor (45 yosh)

- **Maqsad:** Shifokorga tez va oson navbatga yozilish
- **Muammo:** Navbat kutish va qog'ozbozlikdan charchagan
- **Asosiy ehtiyoj:** Mobil orqali 2-3 bosimda navbat olish imkoniyati

### Dr. Alisher — Shifokor (38 yosh)

- **Maqsad:** Bemorlar tarixini tez ko'rish va tashxis qo'yish
- **Muammo:** Ma'lumotlar tarqoqligi va vaqt yetishmasligi
- **Asosiy ehtiyoj:** Yagona EHR tizimi — MKB-10 kodlari va retsept generatsiyasi

### Malika — Registrator (22 yosh)

- **Maqsad:** Navbatlarni tartibga solish va to'lovlarni qabul qilish
- **Muammo:** Telefon qo'ng'iroqlari juda ko'pligi
- **Asosiy ehtiyoj:** Onlayn navbat tizimi orqali ish yukini kamaytirish

---

## 📋 2. Product Backlog & Acceptance Criteria

| ID | User Story | SP | Prioritet | Holat |
|---|---|---|---|---|
| **US.1** | Bemor sifatida OTP bilan ro'yxatdan o'tish | 5 | 🔴 Yuqori | Review/QA |
| **US.2** | AI Simptom Diagnostika chatboti (O'zbek tili) | 13 | 🔴 Yuqori | In Progress |
| **US.3** | Shifokor EHR (Tibbiy karta) moduli | 13 | 🟡 O'rta | Backlog |
| **US.6** | Lab natijalar moduli integratsiyasi | 8 | 🟡 O'rta | Todo |
| **US.7** | Admin KPI va analitika dashboard | 5 | 🟢 Past | Todo |

### US.1 — OTP Login: Acceptance Criteria

1. SMS 60 soniya ichida yetib kelishi kerak
2. Telegram bot orqali OTP olish imkoniyati mavjud bo'lishi kerak
3. 3 marta noto'g'ri kiritilganda avtomatik blokirovka bo'lishi kerak

### US.2 — AI Chatbot: Acceptance Criteria

1. O'zbek tili tibbiy terminlarini 90% aniqlikda tushunishi kerak
2. Kamida 3 ta shifokor mutaxassisligini tavsiya qilishi kerak
3. Shoshilinch holatlarda "Tez yordam" tugmasi ko'rinishi kerak

### US.3 — EHR Moduli: Acceptance Criteria

1. MKB-10 kodlari bo'yicha real-time qidiruv ishlashi kerak
2. Oldingi tashxislar bilan solishtirish grafigi mavjud bo'lishi kerak
3. Retseptni QR kod ko'rinishida generatsiya qilish imkoniyati bo'lishi kerak

---

## 🗂️ 3. Kanban Board — Joriy Sprint

### 📌 Todo (2 ta)

**US.6** — Lab natijalar moduli integratsiyasi `8 SP` `🟡 O'rta`

**US.7** — Admin KPI va analitika dashboard `5 SP` `🟢 Past`

---

### 🔄 In Progress (2 ta)

**US.2** — AI Chatbot RAG Pipeline (Gemini API) `13 SP` `🔴 Yuqori`
> Progress: ████████░░ 65%

**SB.1.2** — Database Schema — Django ORM modellari `3 SP` `🟡 O'rta`
> Progress: ██████████ 80%

---

### 🔍 Review / QA (2 ta)

**US.1** — OTP Login — SMS & Telegram integratsiya `5 SP` `🔴 Yuqori`

**SB.1.4** — Landing Page dizayni (Figma → HTML/CSS) `3 SP` `🟡 O'rta`

---

### ✅ Done (1 ta)

**SB.1.1** — Project Scaffolding — Django + React setup `5 SP` `✓ Tayyor`

---

## 🔄 4. Scrum Lifecycle

```
Product Backlog
      ↓
Sprint Planning  ←─────────────────────────────┐
      ↓                                         │
Sprint Backlog                                  │
      ↓                                         │
  [2 Hafta Sprint]                              │
      ↕                                         │
Daily Standup (har kuni)                        │
      ↓                                         │
Sprint Review (Demo: Jamoa + Stakeholder)       │
      ↓                                         │
Sprint Retrospective ───────────────────────────┘
      ↓
Increment: Shipped Code 🚀
```

---

## 📅 5. Scrum Ceremonies (Marosimlar)

| Marosim | Davomiyligi | Qatnashchilar | Maqsad |
|---|---|---|---|
| **Sprint Planning** | 2–4 soat | Barcha jamoa | Kelgusi 2 hafta uchun vazifalarni tanlash va rejalashtirish |
| **Daily Standup** | 15 daqiqa | Ishlab chiquvchilar | Bugungi reja, kecha nima qilindi va to'siqlarni muhokama qilish |
| **Sprint Review** | 1–2 soat | Jamoa + Stakeholder | Bajarilgan ishni namoyish qilish (Demo) va feedback olish |
| **Sprint Retro** | 1–1.5 soat | Barcha jamoa | Jarayonni qanday yaxshilashni kelishib olish |

---

## ✅ 6. Governance: DoR va DoD

### Definition of Ready (DoR) — Vazifa sprintga tayyor

- [ ] User story aniq yozilgan va Acceptance Criteria mavjud
- [ ] Bog'liqliklar (dependencies) aniqlangan va hal qilingan
- [ ] Story Point (SP) jamoa tomonidan baholangan (Planning Poker)
- [ ] Dizayn prototipi (Figma) tayyor — UI vazifalar uchun
- [ ] Texnik arxitektura yechimi muhokama qilingan

### Definition of Done (DoD) — Vazifa bajarilgan

- [ ] Kod `main` branchga merge bo'lgan
- [ ] GitHub Actions (CI/CD) barcha testlar o'tgan
- [ ] Unit va Integration testlar yozilgan (coverage ≥ 80%)
- [ ] Staging muhitida Manual QA dan o'tgan
- [ ] Hujjatlar (README, API docs) yangilangan
- [ ] Code review kamida 1 jamoa a'zosi tomonidan tasdiqlangan

---

## 👤 7. Jamoa Tarkibi va Rollar

| Rol | Mas'ul | Asosiy vazifa |
|---|---|---|
| **Product Owner** | — | Backlog boshqaruvi, prioritetlar belgilash |
| **Scrum Master** | — | Jarayon oqimini ta'minlash, to'siqlarni olib tashlash |
| **Backend Dev** | — | Django REST API, database, Telegram bot |
| **Frontend Dev** | — | React/TypeScript, UI komponentlar |
| **AI/ML Engineer** | — | Gemini API integratsiyasi, RAG pipeline |
| **QA Engineer** | — | Test yozish, stagingda manual test |

---

## 🛠️ 8. Texnologiyalar Steki

### Backend
- **Framework:** Django 5.x + Django REST Framework
- **Database:** SQLite (dev) → PostgreSQL (prod)
- **AI:** Google Gemini API (RAG pipeline)
- **Bot:** aiogram 3.x (Telegram Mini App)
- **Auth:** OTP via SMS + Telegram

### Frontend
- **Framework:** React 18 + TypeScript
- **Styling:** Tailwind CSS
- **State:** Zustand / React Query
- **Design:** Figma → pixel-perfect implementation

### DevOps
- **CI/CD:** GitHub Actions
- **Hosting:** VPS (Nginx + Gunicorn)
- **Process Manager:** PM2 / Supervisor
- **SSL:** Let's Encrypt

---

## 📈 9. Sprint Burndown — Sprint 1

```
SP  31 |●
       |  \
    25 |    \
       |      ●
    19 |        \
       |          ●  (Joriy holat — kun 7)
    13 |            \
       |              \
     6 |                \
       |                  \
     0 |____________________●
       1   3   5   7   9   11   13   14  (Kun)

       ──── Ideal    ● ● Haqiqiy
```

---

## 🔐 10. Risklar va Muammolar

| Risk | Darajasi | Yechim |
|---|---|---|
| Gemini API so'rovlar chekovi | 🔴 Yuqori | Caching + fallback model qo'shish |
| O'zbek NLP aniqligi < 90% | 🟡 O'rta | Fine-tuning + terminlar lug'ati |
| OTP SMS yetkazib berish | 🟡 O'rta | Backup provider (Telegram OTP) |
| Staging/Prod farqi | 🟢 Past | Docker Compose bilan muhit standartizatsiyasi |

---

## 📌 11. Kelgusi Sprint (Sprint 2) — Rejalashtirilgan

- `US.3` — Shifokor EHR moduli (MKB-10, QR retsept)
- `US.4` — To'lov integratsiyasi (Payme / Click / Uzum)
- `US.5` — Bemor profili va tarix sahifasi
- `SB.2.1` — Production deployment (Nginx + SSL)
- `SB.2.2` — Monitoring va logging tizimi

---

*© 2026 Med-Core Professional · Scrum Maturity Level: 5/5*
*Hujjat oxirgi yangilangan: 2026-mart*
