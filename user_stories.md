# Create user_stories.md with full content
@'
# Patient Appointment Portal — User Stories

> **Conventions**
> - Format: *As a `<role>`, I want `<capability>` so that `<benefit>`*.
> - **Priority**: P1 (must), P2 (should), P3 (nice).
> - **AC** = Acceptance Criteria (Given/When/Then).

## Roles
- **Admin** – manages users, roles, clinic settings.
- **Doctor** – manages availability and appointments.
- **Patient** – books and manages own appointments.

---

## Epic A: Admin Management

### A1. Create & manage users (P1)
**As an** Admin, **I want** to create, edit, deactivate users and assign roles **so that** access is controlled.  
**AC**
- Given I’m an Admin, when I create a user with valid data, then the user appears with the selected role.
- Given a user is active, when I deactivate them, then they cannot sign in.
- Given a user has roles, when I update roles, then permissions update immediately.

### A2. Role-based access control (P1)
**As an** Admin, **I want** predefined roles (Admin/Doctor/Patient) with scoped permissions **so that** least privilege is enforced.  
**AC**
- Given a Patient, when accessing admin screens, then access is denied.
- Given a Doctor, when viewing own schedule, then access is granted; editing other doctors’ schedules is denied.

### A3. Clinic settings (P2)
**As an** Admin, **I want** to define clinic hours, appointment slot length, and max daily bookings **so that** scheduling follows rules.  
**AC**
- Given slot length = 20 min, when patients book, then slots follow 20-min grid within clinic hours.
- Given max daily bookings set, when limit is reached, then further bookings for that doctor/day are blocked.

### A4. Audit log (P2)
**As an** Admin, **I want** an audit trail for critical actions **so that** changes are traceable.  
**AC**
- When a user is created/role changed/appointment deleted, then an audit record with actor, timestamp, and diff is stored.

### A5. Bulk import doctors (P3)
**As an** Admin, **I want** to upload a CSV of doctors **so that** I can onboard quickly.  
**AC**
- Given a valid CSV, when uploaded, then doctors are created; invalid rows are reported with reasons.

---

## Epic B: Doctor Scheduling & Care

### B1. Set availability (P1)
**As a** Doctor, **I want** to define recurring/one-off availability **so that** patients can book only free times.  
**AC**
- Given I add a weekly rule (Mon 09:00–12:00), then slots appear for those periods.
- Given I add an exception (vacation day), then no slots appear on that date.

### B2. View my appointments (P1)
**As a** Doctor, **I want** a calendar of my appointments **so that** I can plan my day.  
**AC**
- Given I’m signed in as a Doctor, when I open “My Schedule,” then I see upcoming appointments with patient name, reason, and status.

### B3. Approve/decline appointments (P1)
**As a** Doctor, **I want** to approve or decline pending bookings **so that** I control my schedule.  
**AC**
- Given an appointment is pending, when I approve, then status becomes “Confirmed” and the patient is notified.
- Given I decline, then the slot is released and the patient is notified with the reason.

### B4. Add visit notes/prescription (P2)
**As a** Doctor, **I want** to record visit notes and prescriptions **so that** medical records are complete.  
**AC**
- Given a confirmed appointment, when I add notes and prescription items, then they are saved to the patient record and visible on next visits.

### B5. Block time (P2)
**As a** Doctor, **I want** to block time for meetings/emergencies **so that** no bookings occur in that period.  
**AC**
- When I create a block from 14:00–15:00, then overlapping patient bookings are prevented.

---

## Epic C: Patient Booking & Self-Service

### C1. Register & sign in (P1)
**As a** Patient, **I want** to register and sign in securely **so that** I can manage my appointments.  
**AC**
- Given valid registration data, when I submit, then my account is created and I receive a verification email.
- Given verified email, when I sign in with correct credentials, then access is granted.

### C2. Search doctor & available slots (P1)
**As a** Patient, **I want** to search doctors by specialty/name and see available times **so that** I can choose a suitable slot.  
**AC**
- Given a specialty filter, when I search, then I see matching doctors and their next available slots.

### C3. Book an appointment (P1)
**As a** Patient, **I want** to book a free slot and specify reason **so that** my visit is scheduled.  
**AC**
- Given a free slot, when I submit booking with reason, then I see “Pending” or “Confirmed” according to the clinic rule, and a notification is sent.

### C4. Reschedule/cancel (P1)
**As a** Patient, **I want** to reschedule or cancel within policy **so that** I can adjust plans.  
**AC**
- Given a confirmed appointment and reschedule ≥24h before, when I pick a new free slot, then the old slot is released and the new one is confirmed/pending.
- Given a cancellation policy, when I cancel inside the restricted window, then I see a warning and (if allowed) proceed with acknowledgment.

### C5. Notifications (P2)
**As a** Patient, **I want** email/SMS reminders **so that** I don’t miss visits.  
**AC**
- When an appointment is created/changed/cancelled, then I receive a notification.
- When the visit is in 24h, then I receive a reminder (configurable).

### C6. View history & prescriptions (P2)
**As a** Patient, **I want** to view my past appointments and prescriptions **so that** I can track treatment.  
**AC**
- Given I open “My History,” then I see previous visits with notes and downloadable prescriptions.

---

## NFRs
Security (RBAC, hashed passwords), Privacy (audit, encryption at rest), Performance (<1s search, <2s booking), Reliability (no double booking), Observability (logs/metrics).

'@ | Set-Content -Encoding UTF8 -NoNewline -Pa
