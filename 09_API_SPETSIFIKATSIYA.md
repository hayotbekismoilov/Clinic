# 09. API Spetsifikatsiyasi (REST API Endpoints)

> **Texnologiya:** NestJS, Swagger/OpenAPI 3.0  
> **Base URL:** `https://api.klinika.uz/api/v1`  
> **Autentifikatsiya:** JWT Bearer Token  

---

## 1.1. Auth (Autentifikatsiya)

| Method | Endpoint | Tavsif | Auth |
|--------|----------|--------|------|
| `POST` | `/auth/send-otp` | OTP kod yuborish | ❌ |
| `POST` | `/auth/verify-otp` | OTP ni tasdiqlash va token olish | ❌ |
| `POST` | `/auth/refresh` | Access token yangilash | 🔑 Refresh |
| `POST` | `/auth/logout` | Tizimdan chiqish | 🔑 JWT |
| `GET`  | `/auth/me` | Hozirgi foydalanuvchi ma'lumoti | 🔑 JWT |

### Misollar:

```json
// POST /auth/send-otp
// Request:
{ "phone": "+998901234567" }

// Response (200):
{ 
  "success": true,
  "message": "OTP kod yuborildi",
  "expires_in": 300,
  "retry_after": 60
}

// POST /auth/verify-otp
// Request:
{ "phone": "+998901234567", "code": "847291" }

// Response (200):
{
  "access_token": "eyJhbGciOiJSUzI1NiIs...",
  "refresh_token": "dGhpcyBpcyBhIHJlZnJlc2g...",
  "token_type": "Bearer",
  "expires_in": 900,
  "user": {
    "id": "PAT-00001234",
    "full_name": "Ahmadov Sherzod",
    "phone": "+998901234567",
    "role": "PATIENT"
  }
}
```

---

## 1.2. Departments (Yo'nalishlar)

| Method | Endpoint | Tavsif | Auth |
|--------|----------|--------|------|
| `GET`  | `/departments` | Barcha yo'nalishlar ro'yxati | ❌ |
| `GET`  | `/departments/:slug` | Bitta yo'nalish batafsil | ❌ |
| `GET`  | `/departments/:slug/doctors` | Yo'nalishdagi shifokorlar | ❌ |
| `POST` | `/departments` | Yo'nalish yaratish | 🔑 Admin |
| `PUT`  | `/departments/:id` | Yo'nalish tahrirlash | 🔑 Admin |
| `DELETE`| `/departments/:id` | Yo'nalish o'chirish | 🔑 Admin |

```json
// GET /departments
// Response (200):
{
  "data": [
    {
      "id": "dept_001",
      "name": "Kardiologiya",
      "slug": "kardiologiya",
      "icon": "heart",
      "description": "Yurak va qon tomir kasalliklari...",
      "doctors_count": 5,
      "avg_rating": 4.8,
      "min_price": 80000
    }
  ],
  "total": 37
}
```

---

## 1.3. Doctors (Shifokorlar)

| Method | Endpoint | Tavsif | Auth |
|--------|----------|--------|------|
| `GET`  | `/doctors` | Shifokorlar ro'yxati (filtrlash bilan) | ❌ |
| `GET`  | `/doctors/:id` | Shifokor profili | ❌ |
| `GET`  | `/doctors/:id/slots` | Bo'sh vaqt slotlari | ❌ |
| `GET`  | `/doctors/:id/reviews` | Shifokor sharhları | ❌ |
| `POST` | `/doctors` | Shifokor qo'shish | 🔑 Admin |
| `PUT`  | `/doctors/:id` | Shifokor tahrirlash | 🔑 Admin |

```json
// GET /doctors?department=kardiologiya&sort=rating&available=today
// Response (200):
{
  "data": [
    {
      "id": "doc_001",
      "full_name": "Dr. Aliyev Xasan Karimovich",
      "department": "Kardiologiya",
      "speciality": "Kardiolog, Oliy toifa",
      "experience_years": 15,
      "rating": 4.9,
      "rating_count": 234,
      "consult_price": 80000,
      "avatar_url": "https://cdn.klinika.uz/doctors/doc_001.jpg",
      "bio": "15 yillik tajriba, 5000+ muvaffaqiyatli qabul...",
      "nearest_available_slot": {
        "date": "2026-03-15",
        "time": "10:00"
      }
    }
  ],
  "total": 5,
  "page": 1,
  "per_page": 10
}

// GET /doctors/doc_001/slots?date=2026-03-15
// Response (200):
{
  "doctor_id": "doc_001",
  "date": "2026-03-15",
  "slots": [
    { "time": "09:00", "status": "AVAILABLE" },
    { "time": "09:30", "status": "BOOKED" },
    { "time": "10:00", "status": "AVAILABLE" },
    { "time": "10:30", "status": "AVAILABLE" },
    { "time": "11:00", "status": "BOOKED" },
    { "time": "14:00", "status": "AVAILABLE" },
    { "time": "15:30", "status": "AVAILABLE" },
    { "time": "16:00", "status": "BLOCKED" }
  ]
}
```

---

## 1.4. Appointments (Navbatlar)

| Method | Endpoint | Tavsif | Auth |
|--------|----------|--------|------|
| `POST` | `/appointments` | Navbatga yozilish | 🔑 Patient |
| `GET`  | `/appointments` | Mening navbatlarim | 🔑 Patient |
| `GET`  | `/appointments/:id` | Navbat tafsilotlari | 🔑 Patient |
| `PUT`  | `/appointments/:id/reschedule` | Vaqt ko'chirish | 🔑 Patient |
| `PUT`  | `/appointments/:id/cancel` | Bekor qilish | 🔑 Patient |
| `PUT`  | `/appointments/:id/confirm` | Tasdiqlash (24h remind) | 🔑 Patient |
| `GET`  | `/appointments/today` | Bugungi navbatlar (shifokor) | 🔑 Doctor |
| `PUT`  | `/appointments/:id/start` | Qabul boshlash | 🔑 Doctor |
| `PUT`  | `/appointments/:id/complete` | Qabul yakunlash | 🔑 Doctor |

```json
// POST /appointments
// Request:
{
  "doctor_id": "doc_001",
  "date": "2026-03-15",
  "time_slot": "10:00",
  "type": "FIRST_VISIT",
  "notes": "Bosh og'rig'i bo'yicha",
  "payment_method": "CLICK"
}

// Response (201):
{
  "id": "apt_00001234",
  "booking_code": "MED-2026-0415",
  "doctor": { "id": "doc_001", "full_name": "Dr. Aliyev Xasan" },
  "date": "2026-03-15",
  "time_slot": "10:00",
  "status": "CONFIRMED",
  "payment": {
    "amount": 80000,
    "paid": 16000,
    "method": "CLICK",
    "status": "PARTIAL",
    "payment_url": "https://my.click.uz/pay/..."
  },
  "address": "Amir Temur ko'chasi, 100, 2-qavat, 204-xona",
  "reminders": {
    "3_days_before": "2026-03-12T09:00:00",
    "24_hours_before": "2026-03-14T10:00:00",
    "2_hours_before": "2026-03-15T08:00:00"
  }
}
```

---

## 1.5. AI Diagnostika

| Method | Endpoint | Tavsif | Auth |
|--------|----------|--------|------|
| `POST` | `/ai/symptom-check` | Simptom tahlili | ❌/🔑 |
| `GET`  | `/ai/quick-symptoms` | Tezkor simptom tugmalari | ❌ |
| `GET`  | `/ai/sessions/:id` | AI suhbat tarixi | 🔑 Patient |
| `GET`  | `/ai/analytics/trends` | AI qidiruv trendlari | 🔑 Admin |

```json
// POST /ai/symptom-check
// Request:
{
  "message": "Boshim qattiq og'riyapti va ko'nglim ayniyapti",
  "session_id": "ai_ses_001",
  "conversation_history": [
    { "role": "assistant", "content": "Salom! Simptomlaringizni yozing." }
  ]
}

// Response (200):
{
  "session_id": "ai_ses_001",
  "primary_department": {
    "id": "dept_002",
    "name": "Nevrologiya",
    "confidence": 0.87,
    "reason": "Bosh og'rig'i va ko'ngil aynishi nevrolog tekshiruvini talab qiladi."
  },
  "secondary_departments": [
    { "name": "Terapevt", "confidence": 0.45 },
    { "name": "Oftalmolog", "confidence": 0.30 }
  ],
  "recommended_doctors": [
    {
      "id": "doc_005",
      "full_name": "Dr. Karimov Anvar",
      "rating": 4.9,
      "nearest_slot": "2026-03-10 14:00"
    }
  ],
  "urgency_level": "MEDIUM",
  "follow_up_questions": [
    "Bosh og'rig'i qachondan beri?",
    "Harorat bormi?"
  ],
  "disclaimer": "Bu AI tavsiyasi — tashxis emas."
}
```

---

## 1.6. Patient Portal

| Method | Endpoint | Tavsif | Auth |
|--------|----------|--------|------|
| `GET`  | `/patients/me` | Mening profilim | 🔑 Patient |
| `PUT`  | `/patients/me` | Profil yangilash | 🔑 Patient |
| `GET`  | `/patients/me/lab-results` | Tahlil natijalari | 🔑 Patient |
| `GET`  | `/patients/me/prescriptions` | Retseptlarim | 🔑 Patient |
| `GET`  | `/patients/me/visits` | Tashrif tarixim | 🔑 Patient |
| `GET`  | `/patients/me/files` | Tibbiy rasmlarim | 🔑 Patient |
| `GET`  | `/patients/me/vitals` | Vital ko'rsatkichlarim | 🔑 Patient |
| `GET`  | `/patients/me/notifications` | Bildirishnomalarim | 🔑 Patient |

---

## 1.7. CRM (Ichki Tizim)

| Method | Endpoint | Tavsif | Auth |
|--------|----------|--------|------|
| `GET`  | `/crm/patients` | Bemorlar ro'yxati | 🔑 Staff |
| `GET`  | `/crm/patients/:id` | Bemor kartochkasi | 🔑 Doctor |
| `POST` | `/crm/visits` | Tashrif yaratish | 🔑 Doctor |
| `PUT`  | `/crm/visits/:id` | Tashrif tahrirlash | 🔑 Doctor |
| `POST` | `/crm/prescriptions` | Retsept yaratish | 🔑 Doctor |
| `POST` | `/crm/lab-orders` | Tahlil buyurtma | 🔑 Doctor |
| `PUT`  | `/crm/lab-orders/:id/results` | Natija kiritish | 🔑 Lab |
| `GET`  | `/crm/inventory` | Ombor holati | 🔑 Admin |
| `POST` | `/crm/inventory/deduct` | Materiallar chiqarish | 🔑 Doctor |

---

## 1.8. Admin / Finance

| Method | Endpoint | Tavsif | Auth |
|--------|----------|--------|------|
| `GET`  | `/admin/dashboard` | Asosiy ko'rsatkichlar | 🔑 Admin |
| `GET`  | `/admin/finance/report` | Moliya hisoboti | 🔑 Admin |
| `GET`  | `/admin/doctors/kpi` | Shifokor KPI lari | 🔑 Admin |
| `GET`  | `/admin/departments/stats` | Yo'nalish statistikasi | 🔑 Admin |
| `GET`  | `/admin/audit-logs` | Tekshiruv jurnali | 🔑 SuperAdmin |
| `POST` | `/admin/users` | Foydalanuvchi qo'shish | 🔑 Admin |
| `PUT`  | `/admin/users/:id/role` | Rol o'zgartirish | 🔑 SuperAdmin |

---

## 1.9. Webhooks / Callbacks

| Method | Endpoint | Tavsif | Auth |
|--------|----------|--------|------|
| `POST` | `/webhooks/click` | Click to'lov callback | IP whitelist |
| `POST` | `/webhooks/payme` | Payme JSONRPC callback | Basic Auth |
| `POST` | `/webhooks/sms-status` | SMS delivery report | API Key |
| `POST` | `/webhooks/telegram` | Telegram bot updates | Token |

---

## 1.10. Error Response Formati

```json
// Standart xatolik formati:
{
  "success": false,
  "error": {
    "code": "APPOINTMENT_SLOT_TAKEN",
    "message": "Bu vaqt allaqachon band qilingan",
    "details": {
      "doctor_id": "doc_001",
      "date": "2026-03-15",
      "time_slot": "10:00",
      "suggested_alternatives": ["10:30", "11:00", "14:00"]
    }
  },
  "timestamp": "2026-03-09T13:45:00Z",
  "request_id": "req_abc123def456"
}

// HTTP Status kodlar:
// 200 — Muvaffaqiyat
// 201 — Yaratildi
// 400 — Noto'g'ri so'rov
// 401 — Autentifikatsiya talab qilinadi
// 403 — Ruxsat yo'q
// 404 — Topilmadi
// 409 — Konflikt (masalan, slot band)
// 422 — Validatsiya xatosi
// 429 — Juda ko'p so'rov (rate limit)
// 500 — Server xatosi
```

---

## 1.11. Pagination Formati

```json
// Barcha ro'yxat API lari uchun standart:
{
  "data": [...],
  "meta": {
    "total": 150,
    "page": 1,
    "per_page": 20,
    "last_page": 8,
    "has_next": true,
    "has_prev": false
  }
}

// Query parametrlari:
// ?page=1&per_page=20&sort=created_at&order=desc
// ?search=Ahmadov&filter[status]=CONFIRMED
```

---

*Keyingi bo'lim: [10_DIAGRAMMALAR_VA_ALGORITMLAR.md](./10_DIAGRAMMALAR_VA_ALGORITMLAR.md)*
