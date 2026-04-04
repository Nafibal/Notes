# System Requirements Specification

Berikut adalah **Requirements Specification versi terbaru (full rewrite)** dalam **bahasa Inggris**, sudah **mengintegrasikan fitur SPP kategori (tiered pricing)** dan menggantikan dokumen requirements sebelumnya. Dokumen ini dapat dianggap sebagai **single source of truth** untuk fase implementasi.

---

# System Requirements Specification

## Functional Requirements

### FR-01 User Authentication

**Description:**  
The system shall allow users to authenticate using credentials and access the system according to their assigned role.

**Acceptance Criteria:**

- Given a registered user, when valid credentials are provided, then the user is authenticated successfully.
    
- Given invalid credentials, when authentication is attempted, then access is denied with an error message.
    
- Authentication shall be required for all protected resources.
    

---

### FR-02 Role-Based Access Control (RBAC)

**Description:**  
The system shall enforce role-based access control for ADMIN and SANTRI roles.

**Acceptance Criteria:**

- Given a SANTRI user, when accessing admin-only endpoints, then access is denied.
    
- Given an ADMIN user, when accessing santri or admin endpoints, then access is granted as permitted.
    
- Role validation shall be enforced at the backend level.
    

---

### FR-03 Santri Management

**Description:**  
The system shall allow administrators to manage santri records.

**Acceptance Criteria:**

- Given valid input, when an admin creates or updates a santri, then the data is persisted.
    
- Each santri shall be associated with exactly one SPP category.
    
- Santri data can be imported and exported using CSV files.
    

---

### FR-04 SPP Category Management

**Description:**  
The system shall allow administrators to manage SPP categories used to determine SPP amounts.

**Acceptance Criteria:**

- Given valid input, when an admin creates or updates an SPP category, then the category is stored.
    
- An SPP category may be activated or deactivated.
    
- An SPP category cannot be deleted if it is assigned to any santri.
    

---

### FR-05 SPP Category Pricing

**Description:**  
The system shall allow administrators to define SPP amounts per category for each billing period.

**Acceptance Criteria:**

- Given a category and billing period, when an admin sets an amount, then the pricing is stored.
    
- Each active category must have exactly one pricing configuration per period.
    
- Pricing changes shall not affect already generated invoices.
    

---

### FR-06 SPP Invoice Generation

**Description:**  
The system shall automatically generate SPP invoices for each santri based on their assigned category and pricing.

**Acceptance Criteria:**

- Given an active billing period, when invoice generation runs, then an invoice is created for each eligible santri.
    
- The invoice amount shall be derived from the santri’s SPP category pricing.
    
- Each generated invoice shall have an initial status of UNPAID.
    
- Invoice amounts shall be immutable after creation.
    

---

### FR-07 View Active SPP Invoices (Santri)

**Description:**  
The system shall allow santri to view their active SPP invoices.

**Acceptance Criteria:**

- Given a santri with no active invoices, when accessing the dashboard, then an informative empty state is displayed.
    
- Given a santri with active invoices, when accessing the dashboard, then invoice cards are displayed with amount, period, and status.
    

---

### FR-08 Upload Payment Proof (Santri)

**Description:**  
The system shall allow santri to upload payment proof for eligible invoices.

**Acceptance Criteria:**

- Given an invoice with status UNPAID or REJECTED, when a santri uploads a valid payment proof, then the invoice status changes to PENDING.
    
- Given an invoice with status PENDING, when upload is attempted, then the action is rejected.
    
- Uploaded files must pass size and file type validation.
    

---

### FR-09 View Payment Status & Rejection Reason (Santri)

**Description:**  
The system shall display the current payment verification status to santri.

**Acceptance Criteria:**

- Given a PENDING invoice, when viewed, then a “Waiting for verification” status is displayed.
    
- Given a REJECTED invoice, when viewed, then the rejection reason is clearly displayed.
    
- Given a PAID invoice, when viewed, then a “Paid” status and verification date are displayed.
    

---

### FR-10 Verify or Reject Payment Proof (Admin)

**Description:**  
The system shall allow administrators to verify or reject uploaded payment proofs.

**Acceptance Criteria:**

- Given a PENDING payment proof, when an admin verifies it, then the invoice status changes to PAID.
    
- Given a PENDING payment proof, when an admin rejects it and provides a reason, then the invoice status changes to REJECTED.
    
- Only invoices with status PENDING may be verified or rejected.
    

---

### FR-11 Payment History

**Description:**  
The system shall provide read-only payment history access.

**Acceptance Criteria:**

- Given a santri, when accessing payment history, then only their own PAID invoices are displayed.
    
- Given an admin, when accessing payment history, then invoices can be filtered by santri, category, or period.
    
- Payment history records shall not be editable.
    

---

### FR-12 In-App Notifications

**Description:**  
The system shall generate non-realtime in-app notifications for important SPP events.

**Acceptance Criteria:**

- Given a new invoice is generated, then a notification is created for the santri.
    
- Given an invoice is verified or rejected, then a notification is created for the santri.
    
- Notifications can be marked as read.
    

---

### FR-13 Data Integrity & State Enforcement

**Description:**  
The system shall enforce strict data integrity and workflow rules.

**Acceptance Criteria:**

- Invoice status transitions must follow the defined state machine.
    
- Invoice amounts shall not be modified after creation.
    
- Payment proofs shall not be deleted by santri.
    
- All validation must be enforced on the backend.
    

---

## Non-Functional Requirements (Checklist)

### Performance

- Page load time on mobile devices shall be less than **3 seconds**.
    
- Average API response time shall be less than **500 ms** for standard operations.
    

---

### Security

- Authentication mechanism: **JWT-based authentication**.
    
- Authorization: **Role-based access control (RBAC)**.
    
- Password hashing: **bcrypt or equivalent industry-standard algorithm**.
    
- Tokens stored using **HttpOnly cookies**.
    
- File uploads validated by size and MIME type.
    

---

### Reliability

- The system shall handle invalid operations gracefully with consistent error responses.
    
- Failed file uploads shall not alter invoice state.
    
- Critical actions shall be logged for auditing.
    

---

### Scalability

- The system shall support at least **1,000 santri users**.
    
- Backend services shall be stateless to support horizontal scaling.
    
- File storage shall be handled by external object storage.
    

---

### Accessibility

- The santri interface shall follow **WCAG 2.1 Level AA** guidelines where reasonably applicable.
    
- The UI shall be optimized for mobile browsers and touch interaction.
    

---

Jika kamu mau, langkah logis berikutnya adalah:

- **Revisi Database Design (final, dengan kategori)**
    
- **Prisma schema final**
    
- **NestJS module & DTO breakdown**
    
- **Test cases dari acceptance criteria**
    

Tentukan langkah selanjutnya, kita lanjut langsung ke level implementasi.