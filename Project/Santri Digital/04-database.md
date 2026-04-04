Berikut adalah **Database Design Document versi terbaru (full rewrite)** dalam **bahasa Inggris**, sudah **mengakomodasi fitur SPP kategori dan tiered pricing**, dan dapat dianggap sebagai **final reference** untuk implementasi (Prisma / SQL migration).

---

# Database Design

## Database Type

- **PostgreSQL**
    
- Chosen for strong relational integrity, transactional guarantees (ACID), and suitability for financial and audit-sensitive data.
    

---

## Tables Overview

### Table: `users`

Stores authentication and authorization data for all system users.

- `id` (PK, UUID)
    
- `email` (VARCHAR, UNIQUE, nullable)
    
- `password_hash` (VARCHAR)
    
- `role` (ENUM: `ADMIN`, `SANTRI`)
    
- `is_active` (BOOLEAN, default true)
    
- `created_at` (TIMESTAMP, UTC)
    
- `updated_at` (TIMESTAMP, UTC)
    

---

### Table: `santri`

Stores santri-specific profile and billing classification.

- `id` (PK, UUID)
    
- `user_id` (FK → `users.id`, UNIQUE)
    
- `nis` (VARCHAR, UNIQUE)
    
- `full_name` (VARCHAR)
    
- `class_name` (VARCHAR)
    
- `spp_category_id` (FK → `spp_categories.id`)
    
- `status` (ENUM: `ACTIVE`, `INACTIVE`)
    
- `created_at` (TIMESTAMP, UTC)
    
- `updated_at` (TIMESTAMP, UTC)
    

---

### Table: `spp_categories`

Defines billing categories for santri.

- `id` (PK, UUID)
    
- `name` (VARCHAR, UNIQUE)
    
- `description` (TEXT, nullable)
    
- `is_active` (BOOLEAN, default true)
    
- `created_at` (TIMESTAMP, UTC)
    
- `updated_at` (TIMESTAMP, UTC)
    

---

### Table: `spp_category_pricing`

Defines SPP amounts per category and billing period.

- `id` (PK, UUID)
    
- `spp_category_id` (FK → `spp_categories.id`)
    
- `period_month` (INT, 1–12)
    
- `period_year` (INT)
    
- `amount` (NUMERIC)
    
- `created_at` (TIMESTAMP, UTC)
    
- `updated_at` (TIMESTAMP, UTC)
    

---

### Table: `spp_invoices`

Represents generated SPP invoices per santri and period.

- `id` (PK, UUID)
    
- `santri_id` (FK → `santri.id`)
    
- `spp_category_id` (FK → `spp_categories.id`)
    
- `period_month` (INT)
    
- `period_year` (INT)
    
- `amount` (NUMERIC) — snapshot from category pricing
    
- `status` (ENUM: `UNPAID`, `PENDING`, `REJECTED`, `PAID`)
    
- `created_at` (TIMESTAMP, UTC)
    
- `updated_at` (TIMESTAMP, UTC)
    

---

### Table: `payment_proofs`

Stores uploaded payment proof metadata.

- `id` (PK, UUID)
    
- `invoice_id` (FK → `spp_invoices.id`)
    
- `file_url` (TEXT)
    
- `file_type` (VARCHAR)
    
- `uploaded_at` (TIMESTAMP, UTC)
    
- `verified_at` (TIMESTAMP, UTC, nullable)
    
- `verified_by` (FK → `users.id`, nullable)
    
- `rejection_reason` (TEXT, nullable)
    
- `status` (ENUM: `PENDING`, `VERIFIED`, `REJECTED`)
    

---

### Table: `notifications`

Stores non-realtime in-app notifications.

- `id` (PK, UUID)
    
- `user_id` (FK → `users.id`)
    
- `type` (VARCHAR)
    
- `message` (TEXT)
    
- `is_read` (BOOLEAN, default false)
    
- `created_at` (TIMESTAMP, UTC)
    

---

### Table: `audit_logs` (Recommended)

Tracks important administrative and system actions.

- `id` (PK, UUID)
    
- `actor_id` (FK → `users.id`)
    
- `action` (VARCHAR)
    
- `entity_type` (VARCHAR)
    
- `entity_id` (UUID)
    
- `created_at` (TIMESTAMP, UTC)
    

---

## Relationships

- `users` 1—1 `santri`
    
- `spp_categories` 1—* `santri`
    
- `spp_categories` 1—* `spp_category_pricing`
    
- `santri` 1—* `spp_invoices`
    
- `spp_invoices` 1—* `payment_proofs`
    
- `users` (ADMIN) 1—* `payment_proofs` (as verifier)
    
- `users` 1—* `notifications`
    
- `users` 1—* `audit_logs`
    

---

## Constraints & Indexes

### Constraints

- `users.email` must be unique when not null.
    
- `santri.nis` must be unique.
    
- Each santri must be assigned exactly one `spp_category`.
    
- Unique constraint on `spp_category_pricing (spp_category_id, period_month, period_year)`.
    
- Unique constraint on `spp_invoices (santri_id, period_month, period_year)`.
    
- Invoice `amount` is immutable after creation.
    
- Only one active `payment_proof` with status `PENDING` per invoice.
    

---

### Indexes

- Index on `users.role`
    
- Index on `santri.nis`
    
- Index on `santri.spp_category_id`
    
- Composite index on `spp_invoices (santri_id, period_month, period_year)`
    
- Index on `spp_invoices.status`
    
- Index on `payment_proofs.invoice_id`
    
- Index on `notifications.user_id`
    

---

## Data Integrity Rules

- Invoice status transitions must follow the defined state machine:  
    `UNPAID → PENDING → PAID / REJECTED → PENDING`
    
- Pricing changes do not retroactively affect existing invoices.
    
- Payment proofs cannot be deleted by santri.
    
- Rejected payment proofs must retain rejection reasons.
    
- All business rules must be enforced at the application layer in addition to database constraints.
    

---

## Notes for Implementation

- UUIDs are recommended for all primary keys.
    
- Monetary values must use `NUMERIC` to avoid floating-point precision issues.
    
- All timestamps should be stored in UTC.
    
- Use database transactions when generating invoices in bulk.
    
- Add application-level validation to prevent missing pricing configurations.
    

---

### Recommended Next Step

Jika kamu ingin lanjut secara langsung ke implementasi, langkah paling natural adalah:

- **Prisma schema final (ready to paste)**
    
- **Mermaid ERD diagram**
    
- **SQL migration draft**
    
- **NestJS entity & repository mapping**
    

Tinggal sebutkan mana yang kamu inginkan.