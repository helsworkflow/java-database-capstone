# User Stories — Patient Appointment Portal

## User Story Template
```markdown
**Title:**
_As a [user role], I want [feature/goal], so that [reason]._

**Acceptance Criteria:**
1. [Criteria 1]
2. [Criteria 2]
3. [Criteria 3]

**Priority:** [High/Medium/Low]
**Story Points:** [Estimated Effort in Points]

**Notes:**
- [Additional information or edge cases]
```

---

## Admin User Stories

### A1 — Admin logs in to manage the platform
```markdown
**Title:** Admin logs in to manage the platform

_As an **Admin**, I want to log into the portal with my username and password, so that I can manage the platform securely._

**Acceptance Criteria:**
1. Given valid credentials, when I submit the login form, then I’m authenticated and redirected to the admin dashboard.
2. Given invalid credentials, when I submit the form, then I see a clear error and remain on the login page.
3. Given RBAC, when logged in as Admin, then I can access admin-only pages; non-admins are denied.

**Priority:** High
**Story Points:** 3

**Notes:**
- Consider account lockout after N failed attempts; “remember me” optional.
```

### A2 — Admin logs out to protect access
```markdown
**Title:** Admin logs out to protect access

_As an **Admin**, I want to log out of the portal, so that unauthorized users cannot access the system from my session._

**Acceptance Criteria:**
1. Clicking Logout invalidates the session and redirects to the login page.
2. Using the back button does not expose protected pages.
3. Session cookie is removed/expired.

**Priority:** High
**Story Points:** 1

**Notes:**
- Idle timeout policy (optional).
```

### A3 — Admin adds a doctor profile
```markdown
**Title:** Admin adds a doctor profile

_As an **Admin**, I want to add doctors to the portal, so that patients can book appointments with them._

**Acceptance Criteria:**
1. Given valid doctor data (name, specialization, contact), when I save, then a doctor profile is created and searchable.
2. Required fields are validated; errors shown inline.
3. An audit record is created for the action.

**Priority:** High
**Story Points:** 3

**Notes:**
- Optional: avatar upload, locations.
```

### A4 — Admin deletes a doctor profile
```markdown
**Title:** Admin deletes a doctor profile

_As an **Admin**, I want to delete a doctor’s profile, so that I can remove inactive staff from the portal._

**Acceptance Criteria:**
1. Given a doctor profile, when I choose Delete and confirm, then the profile is removed or soft-deleted.
2. If future appointments exist, I’m warned and deletion is blocked or requires reassignment.
3. Audit record captures who deleted and when.

**Priority:** Medium
**Story Points:** 2

**Notes:**
- Prefer soft delete to preserve history.
```

### A5 — Admin runs MySQL stored procedure for monthly appointment stats
```markdown
**Title:** Admin runs MySQL stored procedure for monthly appointment stats

_As an **Admin**, I want to run a stored procedure in the MySQL CLI to get the number of appointments per month, so that I can track usage statistics._

**Acceptance Criteria:**
1. Given DB access, when I execute the stored procedure, then it returns counts grouped by month (YYYY-MM, total).
2. When no data exists for a month, then it returns 0 or omits the month as per spec.
3. Procedure is documented with name, params, and sample output.

**Priority:** Medium
**Story Points:** 2

**Notes:**
- Export results to CSV (optional).
```

---

## Patient User Stories

### P1 — Patient can browse doctors without logging in
```markdown
**Title:** Patient can browse doctors without logging in

_As a **Patient**, I want to view a list of doctors without logging in, so that I can explore options before registering._

**Acceptance Criteria:**
1. When I open the Doctors page, then I see basic profiles (name, specialization) and next available times.
2. Protected actions (book, view details) prompt me to sign up/login.
3. Filters by specialization/name work without authentication.

**Priority:** High
**Story Points:** 2

**Notes:**
- Hide contact info if policy requires login.
```

### P2 — Patient signs up with email and password
```markdown
**Title:** Patient signs up with email and password

_As a **Patient**, I want to sign up using my email and password, so that I can book appointments._

**Acceptance Criteria:**
1. Given valid data, when I register, then an account is created and I receive verification (email).
2. Weak passwords are rejected per policy.
3. After verification, I can log in successfully.

**Priority:** High
**Story Points:** 3

**Notes:**
- Consider captcha / rate limiting.
```

### P3 — Patient logs in to manage bookings
```markdown
**Title:** Patient logs in to manage bookings

_As a **Patient**, I want to log into the portal, so that I can manage my bookings._

**Acceptance Criteria:**
1. With correct credentials, I’m redirected to my dashboard.
2. With incorrect credentials, I see an error and remain on login.
3. Only Patient-scoped features are accessible.

**Priority:** High
**Story Points:** 2

**Notes:**
- Support “remember me” (optional).
```

### P4 — Patient logs out to secure account
```markdown
**Title:** Patient logs out to secure account

_As a **Patient**, I want to log out of the portal, so that my account stays secure on shared devices._

**Acceptance Criteria:**
1. Clicking Logout invalidates my session and redirects me to login/home.
2. Back button does not reveal protected pages.
3. Session cookie cleared.

**Priority:** High
**Story Points:** 1

**Notes:**
- Idle timeout policy (optional).
```

### P5 — Patient books a one-hour appointment
```markdown
**Title:** Patient books a one-hour appointment

_As a **Patient**, I want to log in and book an hour-long appointment to consult with a doctor, so that I can receive care._

**Acceptance Criteria:**
1. Given a free 60-minute slot, when I submit booking with reason, then the appointment is created with status Pending/Confirmed per policy.
2. Double booking is prevented; I receive a confirmation/notification.
3. If no 60-minute slot exists, I see alternatives or next availability.

**Priority:** High
**Story Points:** 5

**Notes:**
- Slot grid and clinic hours defined by admin settings.
```

### P6 — Patient views upcoming appointments
```markdown
**Title:** Patient views upcoming appointments

_As a **Patient**, I want to view my upcoming appointments, so that I can prepare accordingly._

**Acceptance Criteria:**
1. My dashboard lists future appointments with date/time, doctor, location/mode, and status.
2. I can access actions allowed by policy (reschedule/cancel).
3. Past appointments appear in a separate history section.

**Priority:** Medium
**Story Points:** 2

**Notes:**
- Add calendar export (optional).
```

---

## Doctor User Stories

### D1 — Doctor logs in to manage appointments
```markdown
**Title:** Doctor logs in to manage appointments

_As a **Doctor**, I want to log into the portal, so that I can manage my appointments._

**Acceptance Criteria:**
1. With valid credentials, I’m redirected to “My Schedule.”
2. RBAC allows only doctor-scoped pages.
3. Invalid credentials show a clear error.

**Priority:** High
**Story Points:** 2

**Notes:**
- MFA (optional).
```

### D2 — Doctor logs out to protect data
```markdown
**Title:** Doctor logs out to protect data

_As a **Doctor**, I want to log out of the portal, so that my professional data stays protected._

**Acceptance Criteria:**
1. Logout invalidates session and redirects to login.
2. Back navigation doesn’t expose protected pages.
3. Session cookie removed.

**Priority:** High
**Story Points:** 1

**Notes:**
- Idle timeout policy (optional).
```

### D3 — Doctor views appointment calendar
```markdown
**Title:** Doctor views appointment calendar

_As a **Doctor**, I want to view my appointment calendar, so that I can stay organized._

**Acceptance Criteria:**
1. Calendar shows upcoming appointments with patient name, reason, and status.
2. Supports day/week/month views; time zone correct.
3. Updates reflect new bookings/cancellations in near-real-time.

**Priority:** High
**Story Points:** 3

**Notes:**
- ICS export (optional).
```

### D4 — Doctor marks unavailability
```markdown
**Title:** Doctor marks unavailability

_As a **Doctor**, I want to mark my unavailability, so that patients can only see and book available slots._

**Acceptance Criteria:**
1. I can block one-off periods and recurring rules; blocked times are not bookable.
2. If blocks overlap existing bookings, I’m warned and must reschedule/confirm override.
3. Patients see only free slots after applying my unavailability.

**Priority:** High
**Story Points:** 3

**Notes:**
- Sync with clinic hours settings.
```

### D5 — Doctor updates profile with specialization and contact
```markdown
**Title:** Doctor updates profile with specialization and contact

_As a **Doctor**, I want to update my profile with specialization and contact info, so that patients have up-to-date information._

**Acceptance Criteria:**
1. I can edit specialization, bio, contact methods; required fields validated.
2. Changes are reflected immediately in patient search/results.
3. Audit captures profile updates.

**Priority:** Medium
**Story Points:** 2

**Notes:**
- Optional: languages spoken, photo.
```

### D6 — Doctor views patient details for upcoming appointments
```markdown
**Title:** Doctor views patient details for upcoming appointments

_As a **Doctor**, I want to view the patient details for upcoming appointments, so that I can be prepared._

**Acceptance Criteria:**
1. For each future appointment, I can open a detail view with patient demographics and reason for visit.
2. Access complies with privacy/RBAC; I cannot see patients not on my schedule.
3. Audit log records each access to patient details.

**Priority:** Medium
**Story Points:** 2

**Notes:**
- Link to past notes/prescriptions (read-only).
```
