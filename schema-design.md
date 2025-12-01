## MySQL Database Design

### Table: patients
- id: INT, Primary Key, AUTO_INCREMENT
- first_name: VARCHAR(100), NOT NULL
- last_name: VARCHAR(100), NOT NULL
- date_of_birth: DATE, NOT NULL
- email: VARCHAR(150), UNIQUE
- phone: VARCHAR(30)
- created_at: DATETIME, DEFAULT CURRENT_TIMESTAMP

**Notes / Constraints**
- Email можно валидировать в Java коде.
- При удалении пациента история назначений должна оставаться.

- ### Table: doctors
- id: INT, Primary Key, AUTO_INCREMENT
- first_name: VARCHAR(100), NOT NULL
- last_name: VARCHAR(100), NOT NULL
- specialization: VARCHAR(150), NOT NULL
- email: VARCHAR(150), UNIQUE
- phone: VARCHAR(30)
- available_from: TIME
- available_to: TIME
- created_at: DATETIME, DEFAULT CURRENT_TIMESTAMP

**Notes**
- В будущем можно добавить расписание по дням.

- ### Table: appointments
- id: INT, Primary Key, AUTO_INCREMENT
- doctor_id: INT, Foreign Key → doctors(id)
- patient_id: INT, Foreign Key → patients(id)
- appointment_time: DATETIME, NOT NULL
- status: INT, NOT NULL  (0 = Scheduled, 1 = Completed, 2 = Cancelled)
- created_at: DATETIME, DEFAULT CURRENT_TIMESTAMP

**Notes**
- Доктор не должен иметь пересекающиеся записи — это можно реализовать в сервисном слое.
- При удалении доктора или пациента назначение можно оставить для истории.

- ### Table: admin_users
- id: INT, Primary Key, AUTO_INCREMENT
- username: VARCHAR(100), UNIQUE, NOT NULL
- password_hash: VARCHAR(255), NOT NULL
- role: VARCHAR(50)  (e.g., 'ADMIN', 'STAFF')
- created_at: DATETIME, DEFAULT CURRENT_TIMESTAMP

**Notes**
- В будушем подключения Spring Security.

- ## MongoDB Collection Design

- ```json
{
  "_id": "ObjectId('64abc123456')",
  "appointmentId": 51,
  "doctorId": 12,
  "patientId": 34,
  "notes": "Patient reported improved symptoms, continue medication.",
  "tags": ["follow-up", "medication"],
  "attachments": [
    {
      "fileName": "blood_test_2025_01_20.pdf",
      "uploadedAt": "2025-01-20T10:22:00"
    }
  ],
  "metadata": {
    "createdAt": "2025-01-20T10:20:00",
    "updatedAt": "2025-01-20T10:28:00"
  }
}

---

**Notes**
- Ссылка по ID, а не полный объект пациента.
- Можно добавлять вложенные документы, массивы, любые дополнительные поля.
- Подходит для данных, структура которых меняется со временем.
