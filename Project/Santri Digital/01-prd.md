# Product Requirements Document (Lean PRD)

## Problem Statement

Islamic boarding schools (pesantren) currently verify student tuition (SPP) payments manually using chat applications, spreadsheets, or paper-based processes. This manual workflow is time-consuming, error-prone, and lacks a centralized source of truth. Administrators struggle to track payment status accurately, while students receive limited visibility and feedback on their payment submissions.

The absence of a structured system results in delayed verification, inconsistent records, and poor transparency for both administrators and students.

---

## Goals

- Provide a centralized web-based system to manage and verify SPP payment proofs efficiently.
    
- Reduce manual workload and human error in SPP verification for pesantren administrators.
    
- Give students clear visibility into their SPP payment status and history through a mobile-friendly interface.
    

---

## Non-Goals

This product explicitly will NOT:

- Process online payments or integrate payment gateways.
    
- Send real-time notifications (e.g. push notifications, SMS, WhatsApp).
    
- Support financial accounting or advanced reporting beyond basic export.
    
- Serve as a general school management system (academic, attendance, grading).
    

---

## Target Users

### 1. Santri (Students)

- Access the system primarily via mobile browsers.
    
- Upload SPP payment proof.
    
- Track payment status and history.
    
- Receive in-app notifications related to SPP.
    

### 2. Admin Pesantren

- Access the system via desktop or tablet.
    
- Manage student data and SPP configurations.
    
- Verify or reject uploaded payment proofs.
    
- Export SPP payment records.
    

---

## Core Features

- **Authentication & Role-Based Access**
    
    - Secure login for Admin and Santri roles.
        
- **Student Management**
    
    - Create, update, import, and export student data.
        
- **SPP Configuration & Scheduling**
    
    - Configure monthly SPP amount and due dates.
        
- **SPP Invoice Generation**
    
    - Automatically generate SPP invoices per student and period.
        
- **Payment Proof Upload (Santri)**
    
    - Upload image or PDF as proof of payment.
        
- **Payment Verification (Admin)**
    
    - Approve or reject payment proofs with rejection notes.
        
- **SPP Status Tracking**
    
    - Clear states: UNPAID, PENDING, REJECTED, PAID.
        
- **Payment History**
    
    - Read-only payment history for both santri and admin.
        
- **In-App Notifications**
    
    - Non-realtime notifications for new invoices and verification results.
        
- **Import / Export**
    
    - CSV-based import/export for student and payment data.
        

---

## Out of Scope (For Now)

- Online payment integration (bank transfer automation, QRIS payment confirmation).
    
- Real-time notifications (WebSocket, push notifications).
    
- Parent/guardian access.
    
- Multi-pesantren (multi-tenant) support.
    
- Mobile native applications (Android / iOS).
    

---

## Success Metrics

- **SPP Verification Time Reduction**  
    Average time from payment proof upload to verification.
    
- **Task Completion Rate**  
    Percentage of students successfully uploading payment proofs without assistance.
    
- **Admin Processing Efficiency**  
    Number of payment proofs verified per admin per day.
    
- **System Reliability**  
    Error rate during upload and verification processes.
    
- **Performance**  
    Page load time on mobile devices (< 3 seconds on average).
    

---

## Assumptions

- Most students have access to smartphones with a camera and internet connection.
    
- Internet connectivity may be unstable; the system must tolerate retries.
    
- Pesantren administrators are moderately familiar with basic web applications.
    
- Monthly SPP amounts are generally consistent per period.
    

---

## Risks

- **Low Digital Literacy Among Users**  
    _Mitigation:_ Simple UI, mobile-first design, clear status indicators, and minimal user actions.
    
- **Incorrect or Fraudulent Payment Proof Uploads**  
    _Mitigation:_ Manual admin verification with rejection reasons and audit trail.
    
- **Operational Resistance to Change**  
    _Mitigation:_ Gradual onboarding, data import support, and clear benefits for admin workload reduction.
    
- **File Storage & Data Security Risks**  
    _Mitigation:_ Use secure object storage, access control, and proper validation on uploads.
    