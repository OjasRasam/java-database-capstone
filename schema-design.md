# Smart Clinic Management System - Database Schema Design

This document outlines the hybrid data tier architecture for the Smart Clinic Management System, leveraging **MySQL** for structured, transactional relationships and **MongoDB** for unstructured, rapidly evolving clinical data.

---

## 1. MySQL Relational Database Design

The relational database handles core administrative, identity, and scheduling workflows. Data integrity is strictly maintained through primary keys, foreign keys, and unique constraints.

### 1.1 `admins` Table
Stores authentication and profile data for system administrators.
* **Columns:**
    * `id`: INT, AUTO_INCREMENT, PRIMARY KEY
    * `username`: VARCHAR(50), UNIQUE, NOT NULL
    * `password`: VARCHAR(255), NOT NULL (Hashed string)
    * `email`: VARCHAR(100), UNIQUE, NOT NULL

### 1.2 `doctors` Table
Maintains medical practitioner registries and professional specializations.
* **Columns:**
    * `id`: INT, AUTO_INCREMENT, PRIMARY KEY
    * `first_name`: VARCHAR(50), NOT NULL
    * `last_name`: VARCHAR(50), NOT NULL
    * `specialization`: VARCHAR(100), NOT NULL
    * `phone`: VARCHAR(15), UNIQUE, NOT NULL
    * `email`: VARCHAR(100), UNIQUE, NOT NULL

### 1.3 `patients` Table
Tracks registered patient demographics and profile accounts.
* **Columns:**
    * `id`: INT, AUTO_INCREMENT, PRIMARY KEY
    * `first_name`: VARCHAR(50), NOT NULL
    * `last_name`: VARCHAR(50), NOT NULL
    * `email`: VARCHAR(100), UNIQUE, NOT NULL
    * `password`: VARCHAR(255), NOT NULL
    * `date_of_birth`: DATE, NOT NULL
    * `phone`: VARCHAR(15), UNIQUE, NOT NULL

### 1.4 `appointments` Table
Manages scheduling windows mapping patients to doctors.
* **Columns:**
    * `id`: INT, AUTO_INCREMENT, PRIMARY KEY
    * `patient_id`: INT, NOT NULL, FOREIGN KEY REFERENCES `patients(id)` ON DELETE CASCADE
    * `doctor_id`: INT, NOT NULL, FOREIGN KEY REFERENCES `doctors(id)` ON DELETE RESTRICT
    * `appointment_date`: DATE, NOT NULL
    * `time_slot`: TIME, NOT NULL (Constrained to 1-hour intervals)
    * `status`: VARCHAR(20), DEFAULT 'CONFIRMED' (e.g., CONFIRMED, CANCELLED, COMPLETED)
* **Constraints:**
    * `UNIQUE(doctor_id, appointment_date, time_slot)`: Restricts double-booking a specific doctor at the same time.

---

## 2. MongoDB Document Collection Design

Clinical operations require flexible schemas because medical notes, symptom checklists, and medication instructions vary widely between visits. We utilize **MongoDB** to store patient prescriptions under a `prescriptions` collection.

### 2.1 Collection: `prescriptions`
Each document captures a detailed clinical encounter, nesting medications and specific diagnostic criteria seamlessly inside a single document.

```json
{
  "_id": {"$oid": "6675ca2e12f4b32a74c8e19a"},
  "appointment_id": 1042,
  "patient_id": 85,
  "doctor_id": 12,
  "visit_date": "2026-06-21T14:30:00Z",
  "diagnosis": "Acute Bronchitis",
  "vitals": {
    "blood_pressure": "120/80",
    "heart_rate_bpm": 76,
    "temperature_celsius": 37.2
  },
  "medications": [
    {
      "drug_name": "Amoxicillin",
      "dosage": "500mg",
      "frequency": "Three times daily",
      "duration_days": 7
    },
    {
      "drug_name": "Guaifenesin",
      "dosage": "600mg",
      "frequency": "Every 12 hours as needed",
      "duration_days": 5
    }
  ],
  "doctor_notes": "Patient should rest and increase fluid intake. Follow up if cough persists past 10 days."
}
