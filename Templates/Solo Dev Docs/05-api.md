# API Contract

## Authentication
Auth method:
- JWT / Session / API Key

---

## Endpoint List

### POST /api/auth/login
Request:
- email: string
- password: string

Response:
- token: string
- user: object

Errors:
- 401 Invalid credentials

---

### POST /api/auth/register
Request:
- ...

Response:
- ...

---

## Error Handling Format
```json
{
  "error": "ERROR_CODE",
  "message": "Human readable message"
}
