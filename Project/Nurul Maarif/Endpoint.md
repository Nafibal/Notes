### Auth

| Method    | Endpoint         | Description                                                                      |     |
| :-------- | :--------------- | :------------------------------------------------------------------------------- | --- |
| **POST**  | `/auth/login`    | Authenticates Admin (via Email) or Student (via NIS). Returns JWT token.         |     |
| **GET**   | `/auth/profile`  | Get the currently logged-in user's details (used after login to hydrate the UI). |     |
| **PATCH** | `/auth/password` | Allow logged-in users to update their own password.                              |     |

---
### Student
_Managed by: Admin (for creation/updates) and Student (for viewing)._

| Method     | Endpoint        | Role           | Description                                                              |
| :--------- | :-------------- | :------------- | :----------------------------------------------------------------------- |
| **POST**   | `/students`     | Admin          | Register a new student. Assigns NIS, password, and **Tuition Category**. |
| **GET**    | `/students`     | Admin          | List all students. Supports filtering (e.g., by class or category).      |
| **GET**    | `/students/:id` | Admin, Student | Get details of a specific student (includes their Tuition Category).     |
| **PATCH**  | `/students/:id` | Admin          | Update student data (e.g., change Class or update Tuition Category).     |
| **DELETE** | `/students/:id` | Admin          | Soft-delete or remove a student record.                                  |

--- 
### admin
_Managed by: Admin (for management)._

| Method     | Endpoint      | Role  | Description                                |
| :--------- | :------------ | :---- | :----------------------------------------- |
| **POST**   | `/admins`     | Admin | Create a new admin account.                |
| **GET**    | `/admins`     | Admin | List all admin accounts.                   |
| **GET**    | `/admins/:id` | Admin | Get details of a specific admin.           |
| **PATCH**  | `/admins/:id` | Admin | Update admin details (e.g., name changes). |
| **DELETE** | `/admins/:id` | Admin | Remove an admin account.                   |

---


