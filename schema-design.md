# Smart Clinic Management System - Database Schema Design

This blueprint establishes a polyglot persistence strategy for the Smart Clinic Management System. Structured operations and transactional schedules are enforced via **MySQL**, while dynamic clinical artifacts are captured within **MongoDB**.

---

## MySQL Database Design

The relational database layer enforces rigid schemas, physical constraints, and transaction rules to guarantee data integrity across identity, system access, and resource scheduling.

### Table: admin
Tracks administrative identity credentials and system roles.
- `id`: INT, Primary Key, Auto Increment
- `username`: VARCHAR(50), Unique, Not Null
- `password`: VARCHAR(255), Not Null *(Stores a hashed character string for data safety)*
- `email`: VARCHAR(100), Unique, Not Null
- `created_at`: TIMESTAMP, Default CURRENT_TIMESTAMP

### Table: patients
Maintains active consumer accounts and core diagnostic demographics.
- `id`: INT, Primary Key, Auto Increment
- `first_name`: VARCHAR(50), Not Null
- `last_name`: VARCHAR(50), Not Null
- `email`: VARCHAR(100), Unique, Not Null *(Validated via regex in Java code prior to write)*
- `password`: VARCHAR(255), Not Null *(Secured hashed credential)*
- `date_of_birth`: DATE, Not Null
- `phone`: VARCHAR(15), Unique, Not Null
- `gender`: VARCHAR(10), Not Null
- `is_active`: TINYINT(1), Default 1 *(Supports soft deletion to preserve clinical compliance history)*

### Table: doctors
Tracks medical staff registers and operational specialization fields.
- `id`: INT, Primary Key, Auto Increment
- `first_name`: VARCHAR(50), Not Null
- `last_name`: VARCHAR(50), Not Null
- `specialization`: VARCHAR(100), Not Null
- `phone`: VARCHAR(15), Unique, Not Null
- `email`: VARCHAR(100), Unique, Not Null
- `license_number`: VARCHAR(50), Unique, Not Null
- `consultation_fee`: DECIMAL(10,2), Not Null

### Table: appointments
Binds resources together under structural transaction blocks.
- `id`: INT, Primary Key, Auto Increment
- `doctor_id`: INT, Foreign Key → doctors(id) ON DELETE RESTRICT
- `patient_id`: INT, Foreign Key → patients(id) ON DELETE CASCADE
- `appointment_time`: DATETIME, Not Null
- `duration_minutes`: INT, Default 60
- `status`: INT, Not Null, Default 0 *(0 = Scheduled, 1 = Completed, 2 = Cancelled)*

#### Architectural Logic & Design Justifications:
* **Overlapping Appointment Prevention:** The database layer enforces a composite constraint `UNIQUE(doctor_id, appointment_time)` to block double-bookings at the engine level. Overlap evaluation logic across varying time scales is managed by the Spring Boot Application Tier before issuing writes.
* **Deletion Cascades:** If a patient is expunged, their appointments cascade (`ON DELETE CASCADE`) to free up scheduling slots. However, if a doctor profile is deleted, the operational system blocks it (`ON DELETE RESTRICT`) to preserve structural auditing records.
* **History Preservation:** Appointment histories are held indefinitely in MySQL to feed statistical usage dashboards and provide audit compliance parameters.

---

## MongoDB Collection Design

Clinical charts, prescriptions, and health summaries fluctuate naturally depending on the medical checkup type. Forcing these variables into a strict SQL structure results in sparse, empty tables filled with NULL values. MongoDB resolves this by storing dynamic, nested document objects.

### Collection: prescriptions
Captures dynamic medical encounter summaries and nested pharmacy order matrices inside a single flexible BSON document structure.

```json
{
  "_id": "ObjectId('6675ca2e12f4b32a74c8e19a')",
  "appointmentId": 51,
  "patientId": 85,
  "patientName": "John Smith",
  "doctorId": 12,
  "encounterDate": "2026-06-21T14:30:00Z",
  "symptoms": [
    "Persistent dry cough",
    "Mild fatigue",
    "Low-grade fever"
  ],
  "vitals": {
    "bloodPressure": "120/80",
    "heartRateBpm": 76,
    "temperatureCelsius": 37.2
  },
  "medications": [
    {
      "drugName": "Amoxicillin",
      "dosage": "500mg",
      "frequency": "Three times daily",
      "durationDays": 7,
      "refillCount": 1
    },
    {
      "drugName": "Guaifenesin Syrup",
      "dosage": "10ml",
      "frequency": "Every 6 hours as needed",
      "durationDays": 5,
      "refillCount": 0
    }
  ],
  "doctorNotes": "Patient shows signs of clear respiratory congestion. Advised lifestyle modifications including extensive hydration. Follow up required if symptoms persist beyond a week.",
  "pharmacy": {
    "name": "Walgreens SF",
    "location": "Market Street",
    "electronicTransmissionStatus": "SENT"
  },
  "schemaVersion": 1
}
