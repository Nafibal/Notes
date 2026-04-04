# API Contract

## General Conventions

- **Protocol:** HTTPS
    
- **API Style:** REST
    
- **Data Format:** JSON (except file uploads: `multipart/form-data`)
    
- **Authentication:** JWT (Access Token)
    
- **Authorization:** Role-Based Access Control (ADMIN, SANTRI)
    
- **Time Format:** ISO 8601 (UTC)
    

---

## Authentication

**Auth Method:**

- JWT-based authentication
    
- Access token sent via:
    
    - `Authorization: Bearer <token>` header
        
    - or HttpOnly cookie (recommended for santri mobile browsers)
        

---

## Error Handling Format

All error responses must follow this structure:

```json
{
  "error": "ERROR_CODE",
  "message": "Human readable message"
}
```

### Common Error Codes

- `UNAUTHORIZED`
    
- `FORBIDDEN`
    
- `VALIDATION_ERROR`
    
- `NOT_FOUND`
    
- `INVALID_STATE`
    
- `INTERNAL_ERROR`
    

---

## Authentication Endpoints

### POST `/api/auth/login`

Authenticate a user and issue a JWT token.

**Request**

```json
{
  "email": "string",
  "password": "string"
}
```

**Response – 200 OK**

```json
{
  "token": "jwt_token_string",
  "user": {
    "id": "uuid",
    "role": "ADMIN | SANTRI",
    "email": "string"
  }
}
```

**Errors**

- `401 UNAUTHORIZED` – Invalid credentials
    

---

### POST `/api/auth/logout`

Invalidate the current user session.

**Response – 200 OK**

```json
{
  "message": "Logged out successfully"
}
```

---

## Santri Endpoints

### GET `/api/me`

Get the authenticated santri profile.

**Authorization:** SANTRI

**Response – 200 OK**

```json
{
  "id": "uuid",
  "nis": "string",
  "full_name": "string",
  "class_name": "string",
  "spp_category": {
    "id": "uuid",
    "name": "string"
  }
}
```

---

### GET `/api/me/invoices/active`

Get active SPP invoices for the authenticated santri.

**Authorization:** SANTRI

**Response – 200 OK**

```json
[
  {
    "id": "uuid",
    "period_month": 3,
    "period_year": 2026,
    "amount": 500000,
    "status": "UNPAID | PENDING | REJECTED",
    "due_date": "2026-03-10"
  }
]
```

---

### GET `/api/me/invoices/:invoiceId`

Get invoice detail.

**Authorization:** SANTRI

**Response – 200 OK**

```json
{
  "id": "uuid",
  "period_month": 3,
  "period_year": 2026,
  "amount": 500000,
  "status": "UNPAID | PENDING | REJECTED | PAID",
  "payment_info": {
    "bank_name": "string",
    "account_number": "string",
    "account_holder": "string",
    "qris_url": "string | null"
  },
  "payment_proof": {
    "file_url": "string",
    "uploaded_at": "2026-03-02T10:00:00Z",
    "rejection_reason": "string | null"
  }
}
```

---

### POST `/api/me/invoices/:invoiceId/payment-proof`

Upload payment proof for an invoice.

**Authorization:** SANTRI  
**Content-Type:** `multipart/form-data`

**Request**

- `file`: image or PDF
    

**Response – 201 Created**

```json
{
  "message": "Payment proof uploaded successfully",
  "status": "PENDING"
}
```

**Errors**

- `400 VALIDATION_ERROR` – Invalid file
    
- `409 INVALID_STATE` – Invoice status does not allow upload
    

---

### GET `/api/me/invoices/history`

Get payment history.

**Authorization:** SANTRI

**Response – 200 OK**

```json
[
  {
    "id": "uuid",
    "period_month": 2,
    "period_year": 2026,
    "amount": 500000,
    "status": "PAID",
    "paid_at": "2026-02-05T09:00:00Z"
  }
]
```

---

## Admin Endpoints

### GET `/api/admin/santri`

List santri records.

**Authorization:** ADMIN

**Query Parameters**

- `category_id` (optional)
    
- `status` (optional)
    

**Response – 200 OK**

```json
[
  {
    "id": "uuid",
    "nis": "string",
    "full_name": "string",
    "class_name": "string",
    "spp_category": "string"
  }
]
```

---

### POST `/api/admin/santri`

Create a santri.

**Authorization:** ADMIN

**Request**

```json
{
  "nis": "string",
  "full_name": "string",
  "class_name": "string",
  "spp_category_id": "uuid"
}
```

---

### GET `/api/admin/spp/categories`

List SPP categories.

**Authorization:** ADMIN

**Response – 200 OK**

```json
[
  {
    "id": "uuid",
    "name": "Regular",
    "is_active": true
  }
]
```

---

### POST `/api/admin/spp/categories`

Create or update an SPP category.

**Authorization:** ADMIN

**Request**

```json
{
  "name": "string",
  "description": "string"
}
```

---

### POST `/api/admin/spp/categories/:categoryId/pricing`

Set category pricing for a billing period.

**Authorization:** ADMIN

**Request**

```json
{
  "period_month": 3,
  "period_year": 2026,
  "amount": 500000
}
```

---

### GET `/api/admin/invoices`

List SPP invoices.

**Authorization:** ADMIN

**Query Parameters**

- `status`
    
- `period_month`
    
- `period_year`
    
- `category_id`
    

---

### PATCH `/api/admin/payment-proofs/:id/verify`

Verify a payment proof.

**Authorization:** ADMIN

**Response – 200 OK**

```json
{
  "status": "PAID"
}
```

---

### PATCH `/api/admin/payment-proofs/:id/reject`

Reject a payment proof.

**Authorization:** ADMIN

**Request**

```json
{
  "reason": "string"
}
```

**Response – 200 OK**

```json
{
  "status": "REJECTED"
}
```

---

## Notifications

### GET `/api/notifications`

Get in-app notifications.

**Response – 200 OK**

```json
[
  {
    "id": "uuid",
    "message": "Your SPP payment has been verified",
    "is_read": false,
    "created_at": "2026-03-05T10:00:00Z"
  }
]
```

---

## Notes

- All state transitions are validated server-side.
    
- Invoice amount is immutable after creation.
    
- API responses should be consistent and predictable.
    
- Pagination should be added for large lists.
    

