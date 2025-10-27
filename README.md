# MIST4610Project1

## Team Name
**Group 7**

## Group Members: 
[Justus Nour](https://github.com/justusnour), [Jackson Boyer](https://github.com/Jackson9812), [Rong Xin Hu](https://github.com/RongX02), [Trey Trotti](https://github.com/treytrotti), [Sophie Yoo](https://github.com/sophieyoo)


## Scenario Description: 
Our team designed a hospital management system that tracks patient care, appointments, billing, and staff operations. The database supports key functions like scheduling, prescriptions, lab tests, and insurance processing. It helps management monitor patient flow, department performance, and financial outcomes.

## Data Model Explanation: 
Our data model represents how a real hospital functions day to day, capturing the connections between patients, doctors, departments, and the many services that keep a hospital running smoothly. At the core of the model is the Patient entity, since nearly every process in the hospital revolves around the patient. Each patient can have multiple medical records that track their diagnoses and treatments, and they can also make many appointments with different doctors. Because an appointment depends on both the patient and the doctor, it forms a many-to-many relationship, showing how patients and doctors interact throughout the care process. Every appointment results in one Bill, which records the cost and payment details, and since each patient’s bill is tied to their insurance provider, there’s a one-to-many relationship between Insurance and Patient, showing that one insurance plan can cover multiple individuals.
Doctors are organized within Departments such as Cardiology, Pediatrics, or Neurology. A department can have many doctors, and each doctor belongs to one department, forming a one-to-many relationship. Within this, some doctors may supervise others, creating a recursive relationship that reflects real hospital hierarchies. Each department also has one designated department head, connecting a one-to-one relationship between Department and Doctor. Departments are also responsible for multiple Rooms, which represent hospital spaces where patients stay during treatment. Because one room can host several patients over time, this creates a one-to-many relationship between Room and Patient.
Beyond daily appointments, doctors can issue Prescriptions or order Lab Tests for their patients. These depend on both the patient and doctor, forming many-to-many relationships, since multiple patients can receive prescriptions or lab tests from multiple doctors. Altogether, this model mirrors the complex but organized structure of a hospital—how patients move through departments, meet with doctors, receive treatments, get billed, and stay in rooms. By capturing all these interactions, the data model helps ensure that every piece of information works together to support efficient and effective patient care.

<img width="619" height="626" alt="Screenshot 2025-10-23 at 2 35 38 PM" src="https://github.com/user-attachments/assets/b9c066d5-f1b9-444a-9d1c-57ac4c06ad78" />

## Relationships:
| #  | Relationship                            | Type                  | Identifying / Non-Identifying | Description                                                    |
| -- | --------------------------------------- | --------------------- | ----------------------------- | -------------------------------------------------------------- |
| 1  | **Insurance → Patient**                 | 1-to-Many             | Non-Identifying               | One insurance policy can cover many patients.                  |
| 2  | **Department → Doctor**                 | 1-to-Many             | Non-Identifying               | A department employs many doctors.                             |
| 3  | **Doctor → Doctor (Supervisor)**        | 1-to-Many (Recursive) | Non-Identifying               | A doctor may supervise multiple junior doctors.                |
| 4  | **Patient ↔ Doctor (via Appointment)**  | Many-to-Many          | Identifying                   | Appointments depend on both the patient and the doctor.        |
| 5  | **Appointment → Bill**                  | 1-to-1                | Identifying                   | Each appointment generates exactly one bill.                   |
| 6  | **Patient → Medical_Record**            | 1-to-Many             | Identifying                   | Each patient can have multiple medical records.                |
| 7  | **Patient ↔ Doctor (via Prescription)** | Many-to-Many          | Identifying                   | Prescriptions depend on both the patient and the doctor.       |
| 8  | **Patient ↔ Doctor (via Lab_Test)**     | Many-to-Many          | Identifying                   | Lab tests depend on both the patient and the doctor.           |
| 9  | **Department → Room**                   | 1-to-Many             | Non-Identifying               | Each department has multiple rooms.                            |
| 10 | **Room → Patient**                      | 1-to-Many             | Non-Identifying               | Each room can hold one or more patients.                       |
| 11 | **Doctor → Department**                 | 1-to-1                | Identifying                   | Each department has one doctor serving as the department head. |

## Data Dictionary

---

## Appointment
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

## Bill
| Column Name  | Description                               | Data Type     | Size | Format     | Key?         |
|--------------|-------------------------------------------|---------------|------|------------|--------------|
| billID       | Unique number indicating the patient's bill number | INT           | —    | —          | PK           |
| patientID    | Patient linked to bill                    | INT           | —    | —          | FK – Patient |
| insuranceID  | Insurance used                            | INT           | —    | —          | FK – Insurance |
| bill_amount  | Amount to be paid                         | DECIMAL(10,2) | —    | —          | —            |
| bill_date    | The date the patient was billed           | DATE          | —    | YYYY/MM/DD | —            |
| bill_status  | Status of the bill, paid or not           | VARCHAR       | 45   | —          | —            |

---

## Department
| Column Name   | Description                 | Data Type | Size | Format | Key? |
|---------------|-----------------------------|-----------|------|--------|------|
| departmentID  | Unique number for department| INT       | —    | —      | PK   |
| dept_name     | Name of the department      | VARCHAR   | 45   | —      | —    |
| dept_floor    | Floor of the department     | VARCHAR   | 45   | —      | —    |
| dept_head     | Name of the department head | VARCHAR   | 45   | —      | —    |

---

## Doctor
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

## Insurance
| Column Name     | Description               | Data Type | Size | Format | Key? |
|-----------------|---------------------------|-----------|------|--------|------|
| insuranceID     | Unique number for insurance | INT     | —    | —      | PK   |
| provider        | Insurance provider name   | VARCHAR   | 45   | —      | —    |
| ins_policynumber| Insurance policy number   | VARCHAR   | 45   | —      | —    |
| ins_coverage    | Insurance coverage type   | VARCHAR   | 45   | —      | —    |

---

## Lab_Test
| Column Name | Description                 | Data Type | Size | Format     | Key?         |
|-------------|-----------------------------|-----------|------|------------|--------------|
| labtestID   | Unique number for the lab test | INT     | —    | —          | PK           |
| patientID   | Patient tested              | INT       | —    | —          | FK – Patient |
| doctorID    | Doctor ordering the test    | INT       | —    | —          | FK – Doctor  |
| test_type   | Type of lab test            | VARCHAR   | 45   | —          | —            |
| test_result | Result of the lab test      | VARCHAR   | 45   | —          | —            |
| test_date   | Date of the lab test        | DATE      | —    | YYYY/MM/DD | —            |

---

## Med_Record
| Column Name     | Description                          | Data Type | Size | Format     | Key?         |
|-----------------|--------------------------------------|-----------|------|------------|--------------|
| recordID        | Unique number for the record         | INT       | —    | —          | PK           |
| med_diagnosis   | The diagnosis of the patient         | VARCHAR   | 45   | —          | —            |
| med_treatment   | The suggested treatment              | VARCHAR   | 45   | —          | —            |
| med_record_date | Date the record was created          | DATE      | —    | YYYY/MM/DD | —            |
| patientID       | Patient linked                       | INT       | —    | —          | FK – Patient |

---

## Patient
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

## Prescription
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

## Room
| Column Name        | Description                                   | Data Type | Size | Format | Key?            |
|--------------------|-----------------------------------------------|-----------|------|--------|-----------------|
| roomID             | Unique number for specific rooms              | INT       | —    | —      | PK              |
| room_number        | The room number                               | VARCHAR   | 45   | —      | —               |
| room_type          | The room type                                 | VARCHAR   | 45   | —      | —               |
| room_availability  | Status (vacant/occupied)                      | VARCHAR   | 45   | —      | —               |
| departmentID       | Department assigned                           | INT       | —    | —      | FK – Department |

## Database Table

<img width="678" height="603" alt="image" src="https://github.com/user-attachments/assets/300ba37d-936e-4852-9813-297a9a86f2ef" />

## SQL Queries

Query 1 shows how many patients the hospital has for each blood type, excluding those with type B or B+. The results are grouped by blood type and ordered by the number of patients in descending order. 

<img width="408" height="245" alt="Screenshot 2025-10-24 at 10 39 08 AM" src="https://github.com/user-attachments/assets/ed16816a-5d03-4294-b19b-38995f22c3b6" />


This query is useful for medical staff to understand the distribution of blood types among patients, which can help in planning for blood supply, managing inventory, and ensuring that the most needed blood types are available for transfusions and emergencies.

Query 2 lists each patient’s ID, name, and the number of medical records they have.

<img width="374" height="270" alt="Screenshot 2025-10-24 at 10 39 47 AM" src="https://github.com/user-attachments/assets/6a7d356b-01af-47ff-b6af-d974482598da" />


Query 2  allows the hospital to identify which patients have the most medical history or frequent visits. This helps staff recognize patients who may need more ongoing care, follow-ups, or specialized attention. Over time, this information could support improved patient tracking systems or wellness programs for those with frequent hospital visits.


Query 3 lists the first name, last name, and drug name for each patient who is prescribed medication that must be taken once daily.

<img width="432" height="268" alt="Screenshot 2025-10-24 at 10 40 03 AM" src="https://github.com/user-attachments/assets/f2893c89-5c2e-4039-857a-f1d11a45047e" />


This query helps hospital staff quickly see which patients are on daily medications, making it easier to keep track of their treatment routines. This allows doctors and nurses to provide timely reminders, check-ins, and support to help patients stay consistent with their prescriptions, improving their overall care and recovery experience.


Query 4 lists the ID, first name, last name, and department ID of all doctors who work in the Cardiology department.

<img width="400" height="304" alt="Screenshot 2025-10-24 at 10 40 22 AM" src="https://github.com/user-attachments/assets/7c491aca-a47e-4a31-b3ca-42f3abc3bfdd" />


This query helps hospital staff quickly see which doctors are part of the Cardiology team. By having this list, it becomes easier to plan schedules, organize department meetings, and make sure that patients who need heart-related care are assigned to the right specialists. It also helps improve coordination within the department, ensuring that doctors can work together efficiently to provide the best possible care for their patients.

Query 5 lists each doctor's name, their department, and the number of direct reports they have, only showing the doctors who are supervisors with at least two other doctors reporting to them.

<img width="367" height="195" alt="Screenshot 2025-10-26 at 5 29 46 PM" src="https://github.com/user-attachments/assets/7aaa6b9e-ce7a-4cb4-87c7-2573d1ffe2b4" />

This query enables hospital managers to easily identify which doctors are responsible for supervising others. Knowing who the supervisors are makes it easier to understand how the hospital’s teams are structured, plan schedules, and keep departments running smoothly. It also helps ensure that doctors managing larger teams get the support and resources they need to lead effectively and provide the best care possible.


Query 6 shows which hospital departments generate the highest average appointment bill amounts, and how many appointments they have handled. 

<img width="317" height="136" alt="Screenshot 2025-10-26 at 5 30 20 PM" src="https://github.com/user-attachments/assets/d09ac056-9479-4ee0-8bfd-b22450969107" />

Query 6 allows a hospital administrator to identify which departments are the most profitable or the most frequently used. This can help the hospital justify an increase in the number of staff as well as better resource allocations to high earning departments. On the other side, low revenue departments will be identified and reviewed for cost-efficiency improvements.

Query 7 shows which patients currently have unpaid bills, and which department they were treated in.

<img width="317" height="210" alt="Screenshot 2025-10-26 at 5 30 47 PM" src="https://github.com/user-attachments/assets/ef97f327-3eb9-45a6-9d00-dd61295af087" />

Query 7 allows a hospital administrator to track patients that have outstanding balances by department. Departments with high unpaid revenue can be identified, and action can be taken to lessen the number of unpaid bills.

Query 8 allows one to find the cost of the patient's bill and organize it based on their insurance provider

<img width="352" height="230" alt="Screenshot 2025-10-26 at 5 31 26 PM" src="https://github.com/user-attachments/assets/7d662d26-90b8-49aa-8c06-4825a86fbe58" />

Query 8 allows a hospital administrator to access the cost of a patient’s bill to then organize that information based on their insurance provider.

Query 9: List the patient name and their diagnoses organized by their blood type and sum the number of diagnoses.

<img width="352" height="196" alt="Screenshot 2025-10-26 at 5 32 03 PM" src="https://github.com/user-attachments/assets/b63064f4-0435-4c5e-936d-617b14e4b432" />

Query 9 allows a hospital administrator to organize patients and their diagnoses by their blood type. It then sums the number of diagnoses to see which is the most and least common.

Query 10: Query 10 lists the doctors name, their patient count, and the total number of prescriptions given out

<img width="424" height="166" alt="Screenshot 2025-10-26 at 5 32 43 PM" src="https://github.com/user-attachments/assets/ea1fa86f-2078-4dc2-87d2-6472c504cbd6" />

Query 10 shows which doctors write the most prescriptions per patient on average.


# Database Information
Database information: Name of the database: ns_Group7 

Additional information: Each query written above is marked in the database using a stored procedure written by me which can be called using this format: CALL TP_Q1; To call the others, replace Q1 with Q2,Q3,Q4,Q5,Q6,Q7,Q8,Q9 OR Q10







