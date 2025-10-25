# MIST-4610-Project-1

## Team Name
**Group 7**

---

## Group Members
- Jackson Boyer  
- Rong Xin Hu  
- Justus Nour  
- Trey Trotti  
- Sophie Yoo  

---

## Scenario Description
Our group chose to design a database for a hospital management system that helps organize and track patient care, staff activity, and billing information. The goal of this database is to store and manage key information about patients, doctors, appointments, treatments, prescriptions, and financial transactions in a way that supports hospital operations and managerial decision-making. The database allows the hospital to record basic details about patients, insurance providers, and the doctors who treat them. Each doctor is assigned to a specific department, and each patient may have multiple appointments with different doctors. During those appointments, doctors can issue prescriptions, request lab tests, and add new entries to the patient’s medical record. Every patient is assigned to a room during their stay, and each visit creates a bill that is linked to their insurance provider. The billing information helps hospital management track outstanding payments and monitor total revenue. This data model captures the essential operational data of a hospital and supports queries that managers may use to make better decisions—such as identifying the most frequently visited departments, tracking doctor workloads, monitoring patient admissions, and reviewing financial performance.

---

## Data Model Explanation
Our data model represents how a real hospital functions day to day, capturing the connections between patients, doctors, departments, and the many services that keep a hospital running smoothly. At the core of the model is the Patient entity, since nearly every process in the hospital revolves around the patient. Each patient can have multiple medical records that track their diagnoses and treatments, and they can also make many appointments with different doctors. Because an appointment depends on both the patient and the doctor, it forms a many-to-many relationship, showing how patients and doctors interact throughout the care process. Every appointment results in one Bill, which records the cost and payment details, and since each patient’s bill is tied to their insurance provider, there’s a one-to-many relationship between Insurance and Patient, showing that one insurance plan can cover multiple individuals.

Doctors are organized within Departments such as Cardiology, Pediatrics, or Neurology. A department can have many doctors, and each doctor belongs to one department, forming a one-to-many relationship. Within this, some doctors may supervise others, creating a recursive relationship that reflects real hospital hierarchies. Each department also has one designated department head, connecting a one-to-one relationship between Department and Doctor. Departments are also responsible for multiple Rooms, which represent hospital spaces where patients stay during treatment. Because one room can host several patients over time, this creates a one-to-many relationship between Room and Patient.

Beyond daily appointments, doctors can issue Prescriptions or order Lab Tests for their patients. These depend on both the patient and doctor, forming many-to-many relationships, since multiple patients can receive prescriptions or lab tests from multiple doctors. Altogether, this model mirrors the complex but organized structure of a hospital—how patients move through departments, meet with doctors, receive treatments, get billed, and stay in rooms. By capturing all these interactions, the data model helps ensure that every piece of information works together to support efficient and effective patient care.


![image_123650291 (1)](https://github.com/user-attachments/assets/24e616a4-e526-4150-82a1-af6313b2c4a1)

# Data Dictionary

# Appointment
| Column Name       | Description                                   | Data Type | Size | Format       | Key?         |
|-------------------|-----------------------------------------------|-----------|------|--------------|--------------|
| apptID            | Unique number indicating the patient's appointment | INT       | —    | —            | PK           |
| patientID         | Links to patient                               | INT       | —    | —            | FK – Patient |
| doctorID          | Doctor assigned                                | INT       | —    | —            | FK – Doctor  |
| app_date          | Date of appointment                            | DATE      | —    | YYYY/MM/DD   | —            |
| app_time          | Time of the appointment                        | VARCHAR   | 45   | HH:MM        | —            |
| app_reason        | Reason for the appointment                     | VARCHAR   | 45   | —            | —            |
| Bill_patientID    | Connects appointment to billing patient        | INT       | —    | —            | FK – Bill    |
| Bill_insuranceID  | Connects appointment to insurance              | INT       | —    | —            | FK – Bill    |
| Bill_billID       | Connects appointment to specific bill          | INT       | —    | —            | FK – Bill    |

---

# Bill
| Column Name  | Description                               | Data Type     | Size | Format     | Key?         |
|--------------|-------------------------------------------|---------------|------|------------|--------------|
| billID       | Unique number indicating the patient's bill number | INT           | —    | —          | PK           |
| patientID    | Patient linked to bill                    | INT           | —    | —          | FK – Patient |
| insuranceID  | Insurance used                            | INT           | —    | —          | FK – Insurance |
| bill_amount  | Amount to be paid                         | DECIMAL(10,2) | —    | —          | —            |
| bill_date    | The date the patient was billed           | DATE          | —    | YYYY/MM/DD | —            |
| bill_status  | Status of the bill, paid or not           | VARCHAR       | 45   | —          | —            |

---

# Department
| Column Name   | Description                 | Data Type | Size | Format | Key? |
|---------------|-----------------------------|-----------|------|--------|------|
| departmentID  | Unique number for department| INT       | —    | —      | PK   |
| dept_name     | Name of the department      | VARCHAR   | 45   | —      | —    |
| dept_floor    | Floor of the department     | VARCHAR   | 45   | —      | —    |
| dept_head     | Name of the department head | VARCHAR   | 45   | —      | —    |

---

# Doctor
| Column Name   | Description                         | Data Type | Size | Format        | Key?            |
|---------------|-------------------------------------|-----------|------|---------------|-----------------|
| doctorID      | Unique number to identify the doctor| INT       | —    | —             | PK              |
| doc_firstname | First name of the doctor            | VARCHAR   | 45   | —             | —               |
| doc_lastname  | Last name of the doctor             | VARCHAR   | 45   | —             | —               |
| doc_specialty | Doctor's specialty                  | VARCHAR   | 45   | —             | —               |
| doc_phone     | Doctor's phone number               | VARCHAR   | 10   | (XXX)XXX-XXXX | —               |
| doc_email     | Doctor's email                      | VARCHAR   | 45   | —             | —               |
| departmentID  | Department they belong to           | INT       | —    | —             | FK – Department |
| dept_head     | Head of the department (ID)         | INT       | —    | —             | FK – Department |
| supervisorID  | Supervisor doctor (ID)              | INT       | —    | —             | FK – Doctor     |

---

# Insurance
| Column Name     | Description               | Data Type | Size | Format | Key? |
|-----------------|---------------------------|-----------|------|--------|------|
| insuranceID     | Unique number for insurance | INT     | —    | —      | PK   |
| provider        | Insurance provider name   | VARCHAR   | 45   | —      | —    |
| ins_policynumber| Insurance policy number   | VARCHAR   | 45   | —      | —    |
| ins_coverage    | Insurance coverage type   | VARCHAR   | 45   | —      | —    |

---

# Lab_Test
| Column Name | Description                 | Data Type | Size | Format     | Key?         |
|-------------|-----------------------------|-----------|------|------------|--------------|
| labtestID   | Unique number for the lab test | INT     | —    | —          | PK           |
| patientID   | Patient tested              | INT       | —    | —          | FK – Patient |
| doctorID    | Doctor ordering the test    | INT       | —    | —          | FK – Doctor  |
| test_type   | Type of lab test            | VARCHAR   | 45   | —          | —            |
| test_result | Result of the lab test      | VARCHAR   | 45   | —          | —            |
| test_date   | Date of the lab test        | DATE      | —    | YYYY/MM/DD | —            |

---

# Med_Record
| Column Name     | Description                          | Data Type | Size | Format     | Key?         |
|-----------------|--------------------------------------|-----------|------|------------|--------------|
| recordID        | Unique number for the record         | INT       | —    | —          | PK           |
| med_diagnosis   | The diagnosis of the patient         | VARCHAR   | 45   | —          | —            |
| med_treatment   | The suggested treatment              | VARCHAR   | 45   | —          | —            |
| med_record_date | Date the record was created          | DATE      | —    | YYYY/MM/DD | —            |
| patientID       | Patient linked                       | INT       | —    | —          | FK – Patient |

---

# Patient
| Column Name      | Description            | Data Type | Size | Format        | Key?         |
|------------------|------------------------|-----------|------|---------------|--------------|
| patientID        | Unique number for patient | INT    | —    | —             | PK           |
| first_name       | First name of patient  | VARCHAR   | 45   | —             | —            |
| last_name        | Last name of patient   | VARCHAR   | 45   | —             | —            |
| patient_dob      | Date of birth          | DATE      | —    | YYYY/MM/DD    | —            |
| patient_gender   | Gender                 | VARCHAR   | 45   | —             | —            |
| patient_phone    | Phone number           | VARCHAR   | 10   | (XXX)XXX-XXXX | —            |
| patient_addy     | Address                | VARCHAR   | 45   | —             | —            |
| emergency_contact| Emergency contact name | VARCHAR   | 45   | —             | —            |
| patient_bloodtype| Blood Type             | VARCHAR   | 45   | —             | —            |
| insuranceID      | Insurance linked       | INT       | —    | —             | FK – Insurance |
| roomID           | Room assigned          | INT       | —    | —             | FK – Room    |

---

# Prescription
| Column Name       | Description                          | Data Type | Size | Format | Key?         |
|-------------------|--------------------------------------|-----------|------|--------|--------------|
| prescriptionID    | Unique number for the prescription   | INT       | —    | —      | PK           |
| patientID         | Patient prescribed                   | INT       | —    | —      | FK – Patient |
| doctorID          | Doctor who prescribed                | INT       | —    | —      | FK – Doctor  |
| drug_name         | Name of the drug                     | VARCHAR   | 45   | —      | —            |
| drug_dosage       | Correct dosage                       | VARCHAR   | 45   | —      | —            |
| drug_frequency    | How often the drug should be taken   | VARCHAR   | 45   | —      | —            |
| drug_instructions | Instructions on how to take the drug | VARCHAR   | 45   | —      | —            |

---

# Room
| Column Name        | Description                                   | Data Type | Size | Format | Key?            |
|--------------------|-----------------------------------------------|-----------|------|--------|-----------------|
| roomID             | Unique number for specific rooms              | INT       | —    | —      | PK              |
| room_number        | The room number                               | VARCHAR   | 45   | —      | —               |
| room_type          | The room type                                 | VARCHAR   | 45   | —      | —               |
| room_availability  | Status (vacant/occupied)                      | VARCHAR   | 45   | —      | —               |
| departmentID       | Department assigned                           | INT       | —    | —      | FK – Department |
