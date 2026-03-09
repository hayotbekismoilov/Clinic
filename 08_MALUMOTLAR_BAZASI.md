# 08. Ma'lumotlar Bazasi Sxemasi (Database Schema)

> **Texnologiya:** PostgreSQL 16 + Prisma ORM  
> **Mas'ul:** Backend Lead + Database Administrator  

---

## 1.1. ER Diagramma (Entity-Relationship)

```
ASOSIY JADVALLAR VA MUNOSABATLAR:

┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│   PATIENT    │     │  DEPARTMENT  │     │   DOCTOR     │
│──────────────│     │──────────────│     │──────────────│
│ id (PK)      │     │ id (PK)      │     │ id (PK)      │
│ full_name    │     │ name         │     │ full_name    │
│ phone (enc)  │     │ slug         │     │ department_id│──┐
│ birth_date   │     │ icon         │     │ speciality   │  │
│ gender       │     │ description  │     │ experience   │  │
│ blood_type   │     │ is_active    │     │ rating       │  │
│ address (enc)│     │ sort_order   │     │ price        │  │
│ allergies    │     └──────┬───────┘     │ schedule     │  │
│ created_at   │            │             │ is_active    │  │
│ updated_at   │            │             └──────┬───────┘  │
└──────┬───────┘            │                    │          │
       │                    └────────────────────┘          │
       │                                                    │
       │ 1:N                                                │
       │     ┌──────────────┐                               │
       ├────▶│  APPOINTMENT │                               │
       │     │──────────────│                               │
       │     │ id (PK)      │                               │
       │     │ patient_id   │──(FK)── PATIENT               │
       │     │ doctor_id    │──(FK)── DOCTOR                │
       │     │ date         │                               │
       │     │ time_slot    │                               │
       │     │ type         │  {FIRST_VISIT, FOLLOW_UP...} │
       │     │ status       │  {PENDING, CONFIRMED...}     │
       │     │ payment_id   │──(FK)── PAYMENT               │
       │     │ ai_session_id│                               │
       │     │ notes        │                               │
       │     │ created_at   │                               │
       │     └──────────────┘                               │
       │                                                    │
       │ 1:N                                                │
       │     ┌──────────────┐                               │
       ├────▶│ MEDICAL_VISIT│                               │
       │     │──────────────│                               │
       │     │ id (PK)      │                               │
       │     │ patient_id   │──(FK)                         │
       │     │ doctor_id    │──(FK)                         │
       │     │ appointment_id──(FK)                         │
       │     │ complaints   │ (enc)                         │
       │     │ examination  │ (enc)                         │
       │     │ diagnosis_code (MKB-10)                      │
       │     │ diagnosis_text (enc)                         │
       │     │ notes (enc)  │                               │
       │     │ duration_min │                               │
       │     │ created_at   │                               │
       │     └──────┬───────┘                               │
       │            │                                       │
       │            │ 1:N                                   │
       │     ┌──────▼───────┐                               │
       │     │ PRESCRIPTION │                               │
       │     │──────────────│                               │
       │     │ id (PK)      │                               │
       │     │ visit_id     │──(FK)                         │
       │     │ patient_id   │──(FK)                         │
       │     │ doctor_id    │──(FK)                         │
       │     │ medications  │ (JSONB)                       │
       │     │ qr_code      │                               │
       │     │ valid_until  │                               │
       │     │ status       │ {ACTIVE, EXPIRED, CANCELLED}  │
       │     │ created_at   │                               │
       │     └──────────────┘                               │
       │                                                    │
       │ 1:N                                                │
       │     ┌──────────────┐                               │
       ├────▶│  LAB_ORDER   │                               │
       │     │──────────────│                               │
       │     │ id (PK)      │                               │
       │     │ patient_id   │──(FK)                         │
       │     │ doctor_id    │──(FK)                         │
       │     │ visit_id     │──(FK)                         │
       │     │ tests        │ (JSONB)                       │
       │     │ barcode      │                               │
       │     │ status       │ {ORDERED → COMPLETED}         │
       │     │ created_at   │                               │
       │     └──────┬───────┘                               │
       │            │                                       │
       │            │ 1:N                                   │
       │     ┌──────▼───────┐                               │
       │     │  LAB_RESULT  │                               │
       │     │──────────────│                               │
       │     │ id (PK)      │                               │
       │     │ lab_order_id │──(FK)                         │
       │     │ test_code    │                               │
       │     │ test_name    │                               │
       │     │ value        │                               │
       │     │ unit         │                               │
       │     │ reference_min│                               │
       │     │ reference_max│                               │
       │     │ is_abnormal  │ (boolean)                     │
       │     │ completed_at │                               │
       │     └──────────────┘                               │
       │                                                    │
       │ 1:N                                                │
       │     ┌──────────────┐                               │
       ├────▶│ MEDICAL_FILE │                               │
       │     │──────────────│                               │
       │     │ id (PK)      │                               │
       │     │ patient_id   │──(FK)                         │
       │     │ visit_id     │──(FK, nullable)               │
       │     │ file_type    │ {XRAY, MRI, UZI, PDF, OTHER} │
       │     │ file_path    │ (MinIO path)                  │
       │     │ file_name    │                               │
       │     │ mime_type    │                               │
       │     │ size_bytes   │                               │
       │     │ uploaded_by  │──(FK)── USER                  │
       │     │ created_at   │                               │
       │     └──────────────┘                               │
       │                                                    │
       │ 1:N                                                │
       │     ┌──────────────┐                               │
       └────▶│   VITAL      │                               │
             │──────────────│                               │
             │ id (PK)      │                               │
             │ patient_id   │──(FK)                         │
             │ bp_systolic  │ (qon bosimi yuqori)           │
             │ bp_diastolic │ (qon bosimi past)             │
             │ heart_rate   │                               │
             │ temperature  │                               │
             │ weight       │                               │
             │ height       │                               │
             │ spo2         │                               │
             │ recorded_by  │──(FK)── USER                  │
             │ recorded_at  │                               │
             └──────────────┘                               │
                                                            │
┌──────────────┐     ┌──────────────┐                       │
│   PAYMENT    │     │  TIME_SLOT   │                       │
│──────────────│     │──────────────│                       │
│ id (PK)      │     │ id (PK)      │                       │
│ patient_id   │     │ doctor_id    │───────────────────────┘
│ appointment_id│    │ date         │
│ amount       │     │ start_time   │
│ paid_amount  │     │ end_time     │
│ method       │     │ status       │ {AVAILABLE, PENDING,
│ provider     │     │              │  BOOKED, BLOCKED}
│ transaction_id│    │ locked_until │ (15min timeout)
│ status       │     │ booked_by    │──(FK)── PATIENT
│ refund_amount│     │ created_at   │
│ created_at   │     └──────────────┘
└──────────────┘

┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│INVENTORY_ITEM│     │ STOCK_LOG    │     │  AUDIT_LOG   │
│──────────────│     │──────────────│     │──────────────│
│ id (PK)      │     │ id (PK)      │     │ id (PK)      │
│ name         │     │ item_id (FK) │     │ actor_id     │
│ category     │     │ quantity     │     │ actor_role   │
│ current_stock│     │ type         │     │ action       │
│ min_stock    │     │ {IN, OUT,    │     │ resource_type│
│ unit         │     │  ADJUSTMENT} │     │ resource_id  │
│ expiry_date  │     │ reason       │     │ ip_address   │
│ supplier     │     │ visit_id(FK) │     │ details (JSON│
│ price        │     │ created_by   │     │ created_at   │
│ location     │     │ created_at   │     └──────────────┘
│ is_active    │     └──────────────┘
└──────────────┘

┌──────────────┐     ┌──────────────┐
│ NOTIFICATION │     │  AI_SESSION  │
│──────────────│     │──────────────│
│ id (PK)      │     │ id (PK)      │
│ recipient_id │     │ patient_id   │ (nullable-anonim)
│ type         │     │ messages     │ (JSONB)
│ channel      │     │ result       │ (JSONB)
│ {SMS,TG,PUSH}│     │ department_id│ (tavsiya)
│ content      │     │ confidence   │
│ status       │     │ urgency      │
│ {PENDING,    │     │ created_at   │
│  SENT,FAILED}│     └──────────────┘
│ sent_at      │
│ created_at   │     ┌──────────────┐
└──────────────┘     │   REVIEW     │
                     │──────────────│
                     │ id (PK)      │
                     │ patient_id   │
                     │ doctor_id    │
                     │ appointment_id│
                     │ rating (1-5) │
                     │ comment      │
                     │ is_anonymous │
                     │ created_at   │
                     └──────────────┘
```

---

## 1.2. Prisma Schema (Asosiy Jadvallar)

```prisma
// schema.prisma — Asosiy modellar

model Patient {
  id          String   @id @default(cuid())
  fullName    String   @map("full_name")
  phone       String   @unique          // AES-256 encrypted
  birthDate   DateTime @map("birth_date")
  gender      Gender
  bloodType   String?  @map("blood_type")
  address     String?                    // AES-256 encrypted
  allergies   String[]
  telegramId  String?  @map("telegram_id")
  avatarUrl   String?  @map("avatar_url")
  isActive    Boolean  @default(true)
  createdAt   DateTime @default(now())
  updatedAt   DateTime @updatedAt

  appointments  Appointment[]
  visits        MedicalVisit[]
  prescriptions Prescription[]
  labOrders     LabOrder[]
  vitals        Vital[]
  files         MedicalFile[]
  reviews       Review[]
  payments      Payment[]
  notifications Notification[]

  @@map("patients")
}

model Doctor {
  id            String   @id @default(cuid())
  fullName      String   @map("full_name")
  phone         String   @unique
  departmentId  String   @map("department_id")
  speciality    String
  experienceYears Int    @map("experience_years")
  rating        Float    @default(0)
  ratingCount   Int      @default(0) @map("rating_count")
  consultPrice  Int      @map("consult_price")  // tiyinlarda
  bio           String?
  avatarUrl     String?  @map("avatar_url")
  isActive      Boolean  @default(true)
  createdAt     DateTime @default(now())

  department    Department @relation(fields: [departmentId], references: [id])
  appointments  Appointment[]
  visits        MedicalVisit[]
  timeSlots     TimeSlot[]
  prescriptions Prescription[]
  reviews       Review[]

  @@map("doctors")
}

model Department {
  id          String   @id @default(cuid())
  name        String   @unique
  slug        String   @unique
  icon        String
  description String
  isActive    Boolean  @default(true)
  sortOrder   Int      @default(0) @map("sort_order")

  doctors     Doctor[]

  @@map("departments")
}

model Appointment {
  id          String            @id @default(cuid())
  patientId   String            @map("patient_id")
  doctorId    String            @map("doctor_id")
  date        DateTime
  timeSlot    String            @map("time_slot")
  type        AppointmentType
  status      AppointmentStatus @default(PENDING)
  notes       String?
  aiSessionId String?           @map("ai_session_id")
  createdAt   DateTime          @default(now())
  updatedAt   DateTime          @updatedAt

  patient     Patient    @relation(fields: [patientId], references: [id])
  doctor      Doctor     @relation(fields: [doctorId], references: [id])
  visit       MedicalVisit?
  payment     Payment?
  review      Review?

  @@unique([doctorId, date, timeSlot])
  @@map("appointments")
}

enum Gender { MALE FEMALE }
enum AppointmentType { FIRST_VISIT FOLLOW_UP ONLINE PROCEDURE }
enum AppointmentStatus { PENDING CONFIRMED COMPLETED CANCELLED NO_SHOW }
```

---

## 1.3. Indekslar va Optimizatsiya

```sql
-- Performance uchun muhim indekslar:

-- Navbatlarni tez qidirish
CREATE INDEX idx_appointments_doctor_date 
  ON appointments (doctor_id, date, time_slot);

CREATE INDEX idx_appointments_patient 
  ON appointments (patient_id, date DESC);

CREATE INDEX idx_appointments_status 
  ON appointments (status) WHERE status IN ('PENDING', 'CONFIRMED');

-- Bemorni telefon orqali tez topish
CREATE UNIQUE INDEX idx_patients_phone 
  ON patients (phone);

-- Lab natijalarini tez yuklash  
CREATE INDEX idx_lab_results_order 
  ON lab_results (lab_order_id, test_code);

-- Audit loglarni sana bo'yicha qidirish
CREATE INDEX idx_audit_logs_date 
  ON audit_logs (created_at DESC);

-- Time slotlarni tez qidirish
CREATE INDEX idx_time_slots_doctor_date 
  ON time_slots (doctor_id, date) WHERE status = 'AVAILABLE';

-- Ombor: muddati o'tayotganlarni topish
CREATE INDEX idx_inventory_expiry 
  ON inventory_items (expiry_date) WHERE expiry_date IS NOT NULL;
```

---

*Keyingi bo'lim: [09_API_SPETSIFIKATSIYA.md](./09_API_SPETSIFIKATSIYA.md)*
