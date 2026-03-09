# 01. Bosh Sahifa va Klinika Brendingi (Frontend)

> **Modul:** Patient-Facing Website  
> **Texnologiya:** Next.js 14+, Tailwind CSS, Framer Motion  
> **Mas'ul:** Frontend Lead Developer  

---

## 1.1. Bosh Sahifa (Landing Page) Tarkibi

### 1.1.1. Hero Section
Foydalanuvchi saytga birinchi marta kirganda ko'radigan yuqori qismi:

| Element | Tavsif | Texnik Yechim |
|---------|--------|---------------|
| **Video Background** | Klinikaning professional 15-soniyalik loop video | `<video>` tag, lazy-loaded, WebM/MP4 format |
| **Slogan** | "Sog'lom Hayot — Sizning Huquqingiz" | Animated text (Framer Motion `staggerChildren`) |
| **CTA Button (Primary)** | "AI Diagnostika bilan Boshlash" | Gradient button, pulse animation |
| **CTA Button (Secondary)** | "Navbatga Yozilish" | Outlined button, hover glow |
| **Trust Badges** | "10,000+ bemor", "37 yo'nalish", "50+ shifokor" | CountUp animatsiyasi bilan |

### 1.1.2. Hero Section Wireframe

```
┌──────────────────────────────────────────────────────────┐
│  [Logo]              Bosh Sahifa | Bo'limlar | Aloqa     │
├──────────────────────────────────────────────────────────┤
│                                                          │
│           🏥 KLINIKA NOMI                                │
│     "Sog'lom Hayot — Sizning Huquqingiz"                │
│                                                          │
│   ┌─────────────────┐  ┌──────────────────┐             │
│   │ 🤖 AI Diagnostika│  │ 📅 Navbat Olish  │             │
│   └─────────────────┘  └──────────────────┘             │
│                                                          │
│   ┌──────┐  ┌──────┐  ┌──────┐                          │
│   │10000+│  │  37  │  │  50+ │                          │
│   │Bemor │  │Yo'nal│  │Shifok│                          │
│   └──────┘  └──────┘  └──────┘                          │
│                                                          │
│  ─── AI Qidiruv Maydoni ──────────────────────          │
│  │ 🔍 Simptomlaringizni yozing...              │         │
│  ──────────────────────────────────────────────          │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

---

## 1.2. 37 ta Yo'nalish Katalogi

### 1.2.1. Yo'nalishlar Ro'yxati

Har bir yo'nalishning alohida sahifasi, ikonkasi va qisqacha tavsifi bo'ladi:

| # | Yo'nalish | Ikonka | Tavsif |
|---|-----------|--------|--------|
| 1 | Kardiologiya | ❤️ | Yurak va qon tomir kasalliklari |
| 2 | Nevrologiya | 🧠 | Asab tizimi kasalliklari |
| 3 | Umumiy Xirurgiya | 🔪 | Jarrohlik amaliyotlari |
| 4 | Ortopediya | 🦴 | Suyak va bo'g'in kasalliklari |
| 5 | Ginekologiya | 👩 | Ayollar salomatligi |
| 6 | Urologiya | 🩺 | Siydik-tanosil tizimi |
| 7 | Otorinolaringologiya (LOR) | 👂 | Quloq, burun, tomoq |
| 8 | Oftalmologiya | 👁️ | Ko'z kasalliklari |
| 9 | Dermatologiya | 🧴 | Teri kasalliklari |
| 10 | Stomatologiya | 🦷 | Tish davolash |
| 11 | Pediatriya | 👶 | Bolalar salomatligi |
| 12 | Endokrinologiya | 🦋 | Gormon tizimi kasalliklari |
| 13 | Gastroenterologiya | 🫄 | Oshqozon-ichak tizimi |
| 14 | Pulmonologiya | 🫁 | O'pka va nafas yo'llari |
| 15 | Nefrologiya | 🫘 | Buyrak kasalliklari |
| 16 | Onkologiya | 🎗️ | Saraton kasalliklari |
| 17 | Gematologiya | 🩸 | Qon kasalliklari |
| 18 | Revmatologiya | 🤲 | Autoimmun kasalliklar |
| 19 | Infektsion kasalliklar | 🦠 | Yuqumli kasalliklar |
| 20 | Allergologiya | 🤧 | Allergik reaksiyalar |
| 21 | Travmatologiya | 🩹 | Shikastlanish va jarohatlar |
| 22 | Anesteziologiya | 💉 | Og'riqsizlantirish |
| 23 | Reanimatologiya | 🚑 | Shoshilinch tibbiy yordam |
| 24 | Fizioterapiya | 🏋️ | Jismoniy davolash usullari |
| 25 | Psixiatriya | 🧩 | Ruhiy salomatlik |
| 26 | Narkologiya | 🚫 | Qaramlik kasalliklari |
| 27 | Ftiziatriya | 🫁 | Sil kasalligi |
| 28 | Proktologiya | 🏥 | To'g'ri ichak kasalliklari |
| 29 | Mammologiya | 🩺 | Ko'krak bezi kasalliklari |
| 30 | Immunologiya | 🛡️ | Immunitet tizimi |
| 31 | Genetika | 🧬 | Irsiy kasalliklar |
| 32 | Geriatriya | 👴 | Keksa yoshdagilar sog'lig'i |
| 33 | Radiologiya | ☢️ | Rentgen va tasvirlash |
| 34 | Laboratoriya diagnostikasi | 🔬 | Tahlillar |
| 35 | Ultratovush diagnostikasi (UZI) | 📡 | UZI tekshiruvlari |
| 36 | Funksional diagnostika | 📊 | EKG, EEG va boshqalar |
| 37 | Reabilitatsiya | 🔄 | Tiklanish dasturlari |

### 1.2.2. Yo'nalish Sahifasi Tarkibi

Har bir yo'nalish sahifasida quyidagi ma'lumotlar ko'rsatiladi:

```
┌──────────────────────────────────────────────────────────┐
│                                                          │
│  ❤️ KARDIOLOGIYA                                         │
│  ───────────────                                         │
│                                                          │
│  📝 Tavsif:                                              │
│  Yurak va qon tomir kasalliklarini tashxislash va        │
│  davolash. EKG, ExoKG, Holter monitoring kabi            │
│  zamonaviy uskunalar yordamida...                        │
│                                                          │
│  👨‍⚕️ Shifokorlar:                                        │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐                    │
│  │ Dr.Aliev│ │Dr.Karim │ │Dr.Saidov│                    │
│  │ ⭐ 4.9  │ │ ⭐ 4.8  │ │ ⭐ 4.7  │                    │
│  │ 15 yil  │ │ 10 yil  │ │  8 yil  │                    │
│  │[Yozilish]│ │[Yozilish]│ │[Yozilish]│                  │
│  └─────────┘ └─────────┘ └─────────┘                    │
│                                                          │
│  🏷️ Xizmatlar va Narxlar:                               │
│  ├── Birinchi konsultatsiya ─── 80,000 so'm              │
│  ├── EKG ─────────────────── 40,000 so'm                 │
│  ├── ExoKG ──────────────── 120,000 so'm                 │
│  └── Holter 24 soat ─────── 200,000 so'm                 │
│                                                          │
│  🔬 Mavjud Uskunalar:                                    │
│  Philips IntelliVue, GE Vivid E95, Holter BTL ...        │
│                                                          │
│  ❓ Ko'p So'raladigan Savollar (FAQ):                    │
│  └── "Kardiolog qabuliga nima olib borish kerak?"        │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

### 1.2.3. Yo'nalishlar Grid Layout

```
Desktop (4 ustun):
┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐
│ ❤️  │ │ 🧠  │ │ 🔪  │ │ 🦴  │
│Kard.│ │Nevr.│ │Xir. │ │Ort. │
└─────┘ └─────┘ └─────┘ └─────┘
┌─────┐ ┌─────┐ ┌─────┐ ┌─────┐
│ 👩  │ │ 🩺  │ │ 👂  │ │ 👁️  │
│Gin. │ │Urol.│ │LOR  │ │Oft. │
└─────┘ └─────┘ └─────┘ └─────┘
...

Tablet (3 ustun):
┌──────┐ ┌──────┐ ┌──────┐
│  ❤️  │ │  🧠  │ │  🔪  │
│Kard. │ │Nevr. │ │Xir.  │
└──────┘ └──────┘ └──────┘

Mobile (2 ustun, horizontal scroll):
┌────────┐ ┌────────┐
│   ❤️   │ │   🧠   │
│ Kard.  │ │ Nevr.  │
└────────┘ └────────┘
```

---

## 1.3. Virtual Tur (360°)

### 1.3.1. Texnik Implementatsiya

| Parametr | Qiymat |
|----------|--------|
| **Texnologiya** | Pannellum.js yoki Three.js (WebGL) |
| **Format** | Equirectangular panorama (6000x3000 px) |
| **Hotspots** | Bosish orqali boshqa xonaga o'tish |
| **Avto-tur** | 60 sekundli avtomatik aylanma tur |
| **Mobile** | Giroscope (telefon burilish) qo'llab-quvvatlanadi |

### 1.3.2. Xonalar Ro'yxati (Virtual Tur)

```
🏥 Virtual Tur Xaritasi:

Kirish → Qabul Xonasi → ─┬── Kutish Zali
                          ├── Registratura
                          ├── 1-Qavat Koridori
                          │    ├── Terapevt Kabineti
                          │    ├── Kardiolog Kabineti
                          │    └── Nevropatolig Kabineti
                          ├── 2-Qavat Koridori
                          │    ├── Operatsiya Xonasi (360°)
                          │    ├── UZI Xonasi
                          │    └── MRT Xonasi
                          ├── 3-Qavat
                          │    ├── VIP Palata
                          │    ├── Standart Palata
                          │    └── Reabilitatsiya Zali
                          └── Laboratoriya
```

### 1.3.3. Panorama Hotspot Tuzilishi

```javascript
// Har bir hotspot uchun ma'lumotlar tuzilishi:
{
  "id": "room_surgery_01",
  "type": "scene",           // scene | info | link
  "coordinates": {
    "pitch": -5.2,
    "yaw": 120.5
  },
  "target": "scene_surgery_main",
  "tooltip": "Operatsiya Xonasiga O'tish",
  "icon": "arrow_forward",
  "infoPanel": {
    "title": "Zamonaviy Operatsiya Xonasi",
    "description": "Karl Storz endoskopik uskunalar bilan jihozlangan",
    "images": ["equipment_01.jpg"],
    "relatedDepartments": ["Umumiy Xirurgiya", "Travmatologiya"]
  }
}
```

---

## 1.4. Interaktiv Xarita

### 1.4.1. Xarita Funksionalligi

```
┌──────────────────────────────────────────────────────────┐
│  🗺️ KLINIKA JOYLASHUVI                                  │
│                                                          │
│  ┌──────────────────────────────────────┐                │
│  │                                      │                │
│  │         [GOOGLE/YANDEX MAP]          │                │
│  │                                      │                │
│  │    📍 Bosh Klinika                   │                │
│  │    📍 Filial #2                      │                │
│  │    📍 Filial #3                      │                │
│  │                                      │                │
│  └──────────────────────────────────────┘                │
│                                                          │
│  📍 Manzil: Toshkent sh., Amir Temur ko'chasi, 100      │
│  🕐 Ish vaqti: Dushanba-Shanba, 08:00 — 20:00          │
│  📞 Telefon: +998 71 123-45-67                          │
│  🚗 Yo'nalish: [Mening Joylashuvimdan Borish]           │
│                                                          │
└──────────────────────────────────────────────────────────┘
```

### 1.4.2. Xarita Qo'shimcha Funksiyalari

- **GeoLocation API:** Foydalanuvchining joriy joylashuvini aniqlash va eng yaqin filialni ko'rsatish.
- **Route Planning:** Google Directions API orqali avtomobil/piyoda/jamoat transporti yo'nalishini hisoblash.
- **Traffic Layer:** Haqiqiy vaqtda tirbandlik ma'lumotlarini ko'rsatish.
- **ETA (Estimated Time of Arrival):** "20 daqiqada yetib borasiz" kabi taxminiy vaqt.

---

## 1.5. Dizayn Tizimi (Design System)

### 1.5.1. Ranglar Palitrasi

```css
/* === MEDICAL TRUST DESIGN TOKENS === */

:root {
  /* Primary Colors */
  --primary-50:  #E8F4FD;   /* Eng yengil ko'k */
  --primary-100: #B8DEF8;
  --primary-200: #88C8F3;
  --primary-300: #58B2EE;
  --primary-400: #389CE9;
  --primary-500: #1A86E4;   /* Asosiy brend rangi */
  --primary-600: #1570C4;
  --primary-700: #105AA4;
  --primary-800: #0B4484;
  --primary-900: #062E64;   /* Eng quyuq ko'k */
  
  /* Accent (Mint / Medical Green) */
  --accent-50:  #E6FBF5;
  --accent-100: #B3F3E0;
  --accent-200: #80EBCB;
  --accent-300: #4DE3B6;
  --accent-400: #26DBA5;   /* Accent rangi */
  --accent-500: #00D394;
  --accent-600: #00B37D;
  --accent-700: #009366;
  
  /* Neutral (Backgrounds & Text) */
  --neutral-50:  #F8FAFC;   /* Page background */
  --neutral-100: #F1F5F9;   /* Card background */
  --neutral-200: #E2E8F0;   /* Borders */
  --neutral-300: #CBD5E1;
  --neutral-400: #94A3B8;   /* Placeholder text */
  --neutral-500: #64748B;   /* Secondary text */
  --neutral-600: #475569;
  --neutral-700: #334155;   /* Body text */
  --neutral-800: #1E293B;   /* Heading text */
  --neutral-900: #0F172A;   /* Darkest text */
  
  /* Semantic Colors */
  --success: #10B981;  /* Muvaffaqiyat */
  --warning: #F59E0B;  /* Ogohlantirish */
  --error:   #EF4444;  /* Xatolik */
  --info:    #3B82F6;  /* Ma'lumot */
  
  /* Gradients */
  --gradient-hero: linear-gradient(135deg, #1A86E4 0%, #00D394 100%);
  --gradient-card: linear-gradient(180deg, #FFFFFF 0%, #F8FAFC 100%);
  --gradient-cta:  linear-gradient(90deg, #1A86E4 0%, #389CE9 50%, #00D394 100%);
}
```

### 1.5.2. Tipografiya

```css
/* === TYPOGRAPHY SYSTEM === */

/* Google Fonts: Inter, Outfit */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&family=Outfit:wght@400;500;600;700;800&display=swap');

:root {
  --font-heading: 'Outfit', sans-serif;
  --font-body:    'Inter', sans-serif;
  
  /* Font Sizes (rem) */
  --text-xs:   0.75rem;    /* 12px */
  --text-sm:   0.875rem;   /* 14px */
  --text-base: 1rem;       /* 16px */
  --text-lg:   1.125rem;   /* 18px */
  --text-xl:   1.25rem;    /* 20px */
  --text-2xl:  1.5rem;     /* 24px */
  --text-3xl:  1.875rem;   /* 30px */
  --text-4xl:  2.25rem;    /* 36px */
  --text-5xl:  3rem;       /* 48px */
  --text-6xl:  3.75rem;    /* 60px — Hero title */
  
  /* Line Heights */
  --leading-tight:  1.25;
  --leading-normal: 1.5;
  --leading-relaxed: 1.75;
}
```

### 1.5.3. Responsive Breakpoints

```css
/* === BREAKPOINTS === */
/*
  Mobile:      0    — 639px   (Default, mobile-first)
  Tablet:      640px — 1023px  (sm: va md:)
  Desktop:     1024px — 1279px (lg:)
  Wide:        1280px+         (xl: va 2xl:)
*/

/* Container Widths */
.container {
  --container-sm:  640px;
  --container-md:  768px;
  --container-lg:  1024px;
  --container-xl:  1280px;
  --container-2xl: 1400px;  /* Max width */
}
```

### 1.5.4. Komponentlar Kutubxonasi

| Komponent | Tavsif | Variantlari |
|-----------|--------|-------------|
| `Button` | Asosiy tugma | `primary`, `secondary`, `ghost`, `danger`, `success` |
| `Card` | Ma'lumot kartochkasi | `default`, `elevated`, `bordered`, `interactive` |
| `Input` | Matn kiritish maydoni | `text`, `search`, `phone`, `password`, `textarea` |
| `Badge` | Status ko'rsatkichi | `success`, `warning`, `error`, `info`, `neutral` |
| `Avatar` | Foydalanuvchi rasmi | `xs`, `sm`, `md`, `lg`, `xl` + `rounded`, `square` |
| `Modal` | Pop-up oyna | `default`, `fullscreen`, `sheet` (mobile) |
| `Toast` | Bildirishnoma | `success`, `error`, `warning`, `info` |
| `Skeleton` | Yuklanmoqda (loading) | `text`, `card`, `avatar`, `table` |
| `Tabs` | Bo'limlar navigatsiyasi | `line`, `pill`, `enclosed` |
| `Table` | Jadval | `default`, `striped`, `hoverable`, `compact` |
| `Dropdown` | Tanlash menyusi | `select`, `multi-select`, `searchable` |
| `Calendar` | Sana tanlash | `single`, `range`, `time-slots` |
| `Rating` | Reyting yulduzchalar | `readonly`, `interactive`, `half-stars` |
| `Stepper` | Bosqichma-bosqich | `horizontal`, `vertical`, `with-description` |

---

## 1.6. Animatsiyalar va Interaktivlik

### 1.6.1. Sahifa Yuklanganda (Page Load)

```
Ketma-ketlik:
1. [0ms]    Logo fade-in
2. [200ms]  Navigation slide-down
3. [400ms]  Hero title — typewriter effect
4. [800ms]  Hero subtitle — fade-up
5. [1000ms] CTA buttons — scale-in with bounce
6. [1200ms] Trust badges — stagger fade-in (har biri 100ms oraliq)
7. [1600ms] AI search bar — slide-up with glow pulse
```

### 1.6.2. Scroll Animatsiyalari

| Element | Animatsiya | Trigger |
|---------|-----------|---------|
| Yo'nalishlar Grid | Staggered fade-in | Viewport'ga kirganda |
| Shifokorlar kartasi | Slide-up + Scale | Scroll 30% |
| Statistika raqamlar | CountUp (0 → real) | Viewport'ga kirganda |
| Testimonials | Carousel auto-play | Har 5 soniyada |
| Footer | Subtle parallax | Continuous scroll |

### 1.6.3. Hover Effektlari

```css
/* Yo'nalish kartasi hover */
.department-card {
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}
.department-card:hover {
  transform: translateY(-8px);
  box-shadow: 0 20px 40px rgba(26, 134, 228, 0.15);
  border-color: var(--primary-400);
}

/* Shifokor kartasi hover */
.doctor-card:hover .doctor-avatar {
  transform: scale(1.05);
  filter: brightness(1.1);
}
.doctor-card:hover .book-btn {
  background: var(--gradient-cta);
  color: white;
}
```

---

## 1.7. SEO va Performance Talablari

### 1.7.1. SEO

| Element | Talab |
|---------|-------|
| **Title Tag** | `{Yo'nalish nomi} — {Klinika nomi} | Toshkent` |
| **Meta Description** | Har bir sahifa uchun noyob, 155-160 belgi |
| **H1** | Har bir sahifada faqat bitta `<h1>` |
| **Schema.org** | `MedicalClinic`, `Physician`, `MedicalProcedure` markup |
| **Open Graph** | Facebook/Telegram shareable kartochkalar |
| **Sitemap** | Avtomatik generatsiya qilinadigan `sitemap.xml` |
| **Robots.txt** | Bemorlar shaxsiy sahifalarini indekslashdan himoya |

### 1.7.2. Performance Metrikalari (Core Web Vitals)

| Metrika | Maqsad | Qanday erishiladi |
|---------|--------|--------------------|
| **LCP** (Largest Contentful Paint) | < 2.5s | Next.js Image optimization, CDN |
| **FID** (First Input Delay) | < 100ms | Code splitting, lazy loading |
| **CLS** (Cumulative Layout Shift) | < 0.1 | Fixed dimensions, font-display: swap |
| **TTFB** (Time to First Byte) | < 600ms | Edge caching (Cloudflare) |

---

## 1.8. Accessibility (Imkoniyatlar Tengligi)

| Standart | Implementatsiya |
|----------|-----------------|
| **WCAG 2.1 AA** | Rang kontrasti kamida 4.5:1 |
| **Keyboard Nav** | Tab orqali barcha elementlarga o'tish |
| **Screen Reader** | ARIA labels barcha interaktiv elementlarda |
| **Font Size** | Minimum 16px body text |
| **Alt Text** | Barcha rasmlarda tavsif |
| **Focus Ring** | Aniq ko'rinadigan focus indikatori |

---

*Keyingi bo'lim: [02_AI_SIMPTOM_DIAGNOSTIKA.md](./02_AI_SIMPTOM_DIAGNOSTIKA.md)*
