
CREATE DATABASE HMS;
USE HMS;

Patients: Store patient details including name, age, gender, contact information, and medical history.
Doctors: Store information about doctors, their specialization, contact info, and schedules.
Appointments: Record patient appointments with doctors, including appointment time, reason, and status.
CREATE TABLE Insurance_Providers (
    insurance_provider_id INT AUTO_INCREMENT PRIMARY KEY,
    provider_name VARCHAR(255) NOT NULL,   
    contact_number VARCHAR(15),            
    address TEXT,                           
    email VARCHAR(100),                     
    website VARCHAR(100)                    
);
CREATE TABLE Patients (
    patient_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    dob DATE NOT NULL,
    gender VARCHAR(10) NOT NULL,
    phone_number VARCHAR(20) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    address TEXT,
    insurance_provider VARCHAR(100)
);

Bills: Handle billing details for patient treatments, including charges for consultations, tests, and medication.
Staff: Store information about hospital staff such as nurses, technicians, and other support staff.   etc
INSERT INTO Patients (patient_id, first_name, last_name, dob, gender, phone_number, email, address, insurance_provider)
VALUES 
    (1, 'John', 'sharma', '1985-04-12', 'Male', '1234567890', 'johndoe@example.com', '123 Main St', 'XYZ Insurance'),
    (2, 'Alice', 'Smith', '1990-02-25', 'Female', '0987654321', 'alicesmith@example.com', '456 Oak St', 'ABC Insurance'),
    (3, 'Bob', 'Johnson', '1978-08-30', 'Male', '1122334455', 'bobjohnson@example.com', '789 Pine St', 'XYZ Insurance'),
    (4, 'Emily', 'Brown', '1995-11-11', 'Female', '2233445566', 'emilybrown@example.com', '101 Maple St', 'DEF Insurance'),
    (5, 'Michael', 'Davis', '1982-03-17', 'Male', '3344556677', 'michaeldavis@example.com', '202 Birch St', 'XYZ Insurance'),
    (6, 'Sophia', 'Miller', '1988-07-22', 'Female', '4455667788', 'sophiamiller@example.com', '303 Cedar St', 'ABC Insurance'),
    (7, 'James', 'Wilson', '1975-06-12', 'Male', '5566778899', 'jameswilson@example.com', '404 Elm St', 'XYZ Insurance');

CREATE TABLE Insurance (
    insurance_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,                          
    insurance_provider_id INT,               
    policy_number VARCHAR(100) NOT NULL,     
    coverage_details TEXT,                  
    valid_from DATE,                         
    valid_until DATE,                        
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id), 
    FOREIGN KEY (insurance_provider_id) REFERENCES           Insurance_Providers(insurance_provider_id) 
);

INSERT INTO Insurance_Providers (provider_name, contact_number, address, email, website)
VALUES ('Blue Cross', '555-123-4567', '123 Blue St, Springfield', 'contact@bluecross.com', 'https://www.bluecross.com'),
    ('Aetna', '555-987-6543', '456 Aetna Ave, Springfield', 'info@aetna.com', 'https://www.aetna.com');

INSERT INTO Insurance (patient_id, insurance_provider_id, policy_number, coverage_details, valid_from, valid_until)
VALUES (1, 1, 'BC12345', '80% coverage for hospitalization, 100% for emergency services', '2023-01-01', '2024-12-31'),
    (2, 2, 'AET98765', '70% coverage for outpatient, 90% for surgery', '2023-06-15', '2024-06-15');

SELECT p.first_name, p.last_name, i.policy_number, i.coverage_details, i.valid_from, i.valid_until, ip.provider_name
FROM Insurance i
JOIN Patients p ON i.patient_id = p.patient_id
JOIN Insurance_Providers ip ON i.insurance_provider_id = ip.insurance_provider_id
WHERE p.patient_id = 1;

2. Get All Insurance Providers
SELECT * FROM Insurance_Providers;

CREATE TABLE Departments (
  department_id INT AUTO_INCREMENT PRIMARY KEY,
  department_name VARCHAR(100) NOT NULL,
  department_head VARCHAR(50)
);

INSERT INTO Departments (department_name, department_head)
VALUES 
  ('Cardiology', 'Dr. John Taylor'),
  ('Orthopedics', 'Dr. Emily Evans'),
  ('Neurology', 'Dr. Sarah Harris'),
  ('Pediatrics', 'Dr. David Martin'),
  ('Dermatology', 'Dr. Lisa Clark'),
  ('General Surgery', 'Dr. Robert Lewis'),
  ('Gynecology', 'Dr. Olivia Walker');

CREATE TABLE Doctors (
    doctor_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    specialization VARCHAR(100) NOT NULL,
    phone_number VARCHAR(20) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES Departments(department_id)
);
INSERT INTO Doctors (first_name, last_name, specialization, phone_number, email, department_id)
VALUES 
    ('Dr. John', 'Taylor', 'Cardiology', '9876543210', 'johnsharma@hospital.com', 1),
    ('Dr. Emily', 'Evans', 'Orthopedics', '8765432109', 'emilyevans@hospital.com', 2),
    ('Dr. Sarah', 'Harris', 'Neurology', '7654321098', 'sarahharris@hospital.com', 3),
    ('Dr. David', 'Martin', 'Pediatrics', '6543210987', 'davidmartin@hospital.com', 4),
    ('Dr. Lisa', 'Clark', 'Dermatology', '5432109876', 'lisaclark@hospital.com', 5),
    ('Dr. Robert', 'Lewis', 'General Surgery', '4321098765', 'robertlewis@hospital.com', 6),
    ('Dr. Olivia', 'Walker', 'Gynecology', '3210987654', 'oliviawalker@hospital.com', 7);

Check the doctor_id values:
SELECT doctor_id, first_name, last_name FROM Doctors;

CREATE TABLE Appointments (
    appointment_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    doctor_id INT,
    appointment_date DATETIME NOT NULL,
    status VARCHAR(50) NOT NULL,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id),
    FOREIGN KEY (doctor_id) REFERENCES Doctors(doctor_id)
);

INSERT INTO Appointments (patient_id, doctor_id, appointment_date, status)
VALUES 
    (1, 1, '2025-02-20 09:00:00', 'Scheduled'),
    (2, 2, '2025-02-21 10:00:00', 'Scheduled'),
    (3, 3, '2025-02-22 11:00:00', 'Scheduled'),
    (4, 4, '2025-02-23 14:00:00', 'Scheduled'),
    (5, 5, '2025-02-24 15:00:00', 'Scheduled'),
    (6, 6, '2025-02-25 16:00:00', 'Scheduled'),
    (7, 7, '2025-02-26 17:00:00', 'Scheduled');

CREATE TABLE Medications (
    medication_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    manufacturer VARCHAR(100)
);

INSERT INTO Medications (name, description, price)
VALUES ('Aspirin', 'Pain reliever and anti-inflammatory', 10.00),
('Ibuprofen', 'Pain reliever and anti-inflammatory', 12.00),
('Amoxicillin', 'Antibiotic for infections', 25.00),
('Paracetamol', 'Fever and pain relief', 5.00),
('Metformin', 'Diabetes medication', 18.00),
('Lisinopril', 'High blood pressure medication', 20.00),
('Omeprazole', 'Acid reflux medication', 15.00);

CREATE TABLE Prescriptions (
    prescription_id INT AUTO_INCREMENT PRIMARY KEY,
    appointment_id INT,
    medication_id INT,
    dose VARCHAR(50) NOT NULL,
    quantity INT NOT NULL,
    FOREIGN KEY (appointment_id) REFERENCES Appointments(appointment_id),
    FOREIGN KEY (medication_id) REFERENCES Medications(medication_id)
);

INSERT INTO Prescriptions (appointment_id, medication_id, dose, quantity)
VALUES (1, 1, '500mg', 2),
(2, 2, '200mg', 1),
(3, 3, '250mg', 3),
(4, 4, '500mg', 1),
(5, 5, '500mg', 2),
(6, 6, '10mg', 1),
(7, 7, '20mg', 1);

CREATE TABLE Bills (
    bill_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    total_amount DECIMAL(10, 2) NOT NULL,
    bill_date DATE NOT NULL,
    status VARCHAR(50) NOT NULL,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
);

-- Insert Sample Data
INSERT INTO Bills (patient_id, total_amount, bill_date, status)    // YOU can insert like this also 
VALUES (1, 150.00, '2025-02-20', 'Paid');
INSERT INTO Bills (patient_id, total_amount, bill_date, status)
VALUES (2, 200.00, '2025-02-21', 'Pending');
INSERT INTO Bills (patient_id, total_amount, bill_date, status)
VALUES (3, 180.00, '2025-02-22', 'Paid');
INSERT INTO Bills (patient_id, total_amount, bill_date, status)
VALUES (4, 220.00, '2025-02-23', 'Pending');
INSERT INTO Bills (patient_id, total_amount, bill_date, status)
VALUES (5, 250.00, '2025-02-24', 'Paid');
INSERT INTO Bills (patient_id, total_amount, bill_date, status)
VALUES (6, 210.00, '2025-02-25', 'Pending');
INSERT INTO Bills (patient_id, total_amount, bill_date, status)
VALUES (7, 230.00, '2025-02-26', 'Paid');

CREATE TABLE Payments (
    payment_id INT AUTO_INCREMENT PRIMARY KEY,
    bill_id INT,
    payment_date DATE NOT NULL,
    amount DECIMAL(10, 2) NOT NULL,
    payment_method VARCHAR(50) NOT NULL,
    FOREIGN KEY (bill_id) REFERENCES Bills(bill_id)
);

Insert Sample Data
INSERT INTO Payments (bill_id, payment_date, amount, payment_method)
VALUES (1, '2025-02-20', 150.00, 'Credit Card'),
(2, '2025-02-22', 100.00, 'Cash'),
(3, '2025-02-22', 180.00, 'Debit Card'),
(4, '2025-02-24', 220.00, 'Credit Card'),
(5, '2025-02-25', 250.00, 'Bank Transfer'),
(6, '2025-02-26', 150.00, 'Cash'),
(7, '2025-02-27', 230.00, 'Debit Card');

CREATE TABLE Medical_History (
    history_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    disease VARCHAR(100) NOT NULL,
    diagnosis_date DATE NOT NULL,
    treatment_details TEXT,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
);

INSERT INTO Medical_History (patient_id, disease, diagnosis_date, treatment_details)
VALUES (1, 'Hypertension', '2025-01-10', 'Prescribed beta blockers and lifestyle changes'),
(2, 'Asthma', '2025-02-01', 'Inhaler prescribed for acute episodes'),
(3, 'Diabetes Type 2', '2025-02-05', 'Insulin therapy and dietary changes'),
(4, 'Flu', '2025-01-25', 'Rest, fluids, and antiviral medications'),
(5, 'Anxiety Disorder', '2025-02-10', 'Counseling and SSRIs'),
(6, 'Migraine', '2025-02-14', 'Triptans prescribed and migraine management plan'),
(7, 'Osteoarthritis', '2025-02-18', 'Physical therapy and pain management');

CREATE TABLE Nurses (
    nurse_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    shift_time VARCHAR(50) NOT NULL,
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES Departments(department_id)
);

-- Insert Sample Data
INSERT INTO Nurses (first_name, last_name, shift_time, department_id)
VALUES ('Mary', 'Green', 'Morning', 1),
('Olivia', 'White', 'Evening', 2),
('Sophia', 'Black', 'Night', 3),
('Charlotte', 'Martin', 'Morning', 4),
('Ava', 'Lee', 'Evening', 5),
('Isabella', 'King', 'Night', 6),
 ('Mia', 'Scott', 'Morning', 7);


CREATE TABLE Lab_Tests (
    test_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    test_name VARCHAR(100) NOT NULL,
    test_date DATE NOT NULL,
    result TEXT,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
);

INSERT INTO Lab_Tests (patient_id, test_name, test_date, result)
VALUES
(1, 'Blood Pressure Test', '2025-01-15', 'Normal'),
(2, 'Lung Function Test', '2025-02-03', 'Normal'),
(3, 'Blood Glucose Test', '2025-02-08', 'High'),
(4, 'Flu Test', '2025-01-26', 'Positive'),
(5, 'Mental Health Evaluation', '2025-02-12', 'Anxiety Diagnosed'),
(6, 'Migraine Frequency Test', '2025-02-16', 'Moderate Frequency'),
(7, 'X-Ray', '2025-02-20', 'Osteoarthritis Detected');

CREATE TABLE Test_Results (
    result_id INT AUTO_INCREMENT PRIMARY KEY,
    test_id INT,
    result_value VARCHAR(100) NOT NULL,
    result_status VARCHAR(50) NOT NULL,
    FOREIGN KEY (test_id) REFERENCES Lab_Tests(test_id)
);

INSERT INTO Test_Results (test_id, result_value, result_status)
VALUES
(1, '120/80 mmHg', 'Normal'),
(2, '90% Lung Capacity', 'Normal'),
(3, '150 mg/dL', 'High'),
(4, 'Positive for Influenza A', 'Positive'),
(5, 'Generalized Anxiety Disorder', 'Diagnosed'),
(6, 'Frequency: 3-4 per month', 'Moderate'),
(7, 'Joint Degeneration Detected', 'Positive');

CREATE TABLE Surgical_Procedures (
    procedure_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    doctor_id INT,
    procedure_name VARCHAR(100) NOT NULL,
    procedure_date DATE NOT NULL,
    status VARCHAR(50) NOT NULL,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id),
    FOREIGN KEY (doctor_id) REFERENCES Doctors(doctor_id)
);

INSERT INTO Surgical_Procedures (patient_id, doctor_id, procedure_name, procedure_date, status)
VALUES (1, 1, 'Coronary Artery Bypass', '2025-03-01', 'Scheduled');
INSERT INTO Surgical_Procedures (patient_id, doctor_id, procedure_name, procedure_date, status)
VALUES (2, 2, 'Knee Replacement', '2025-03-02', 'Completed');
INSERT INTO Surgical_Procedures (patient_id, doctor_id, procedure_name, procedure_date, status)
VALUES (3, 3, 'Appendectomy', '2025-03-03', 'Scheduled');
INSERT INTO Surgical_Procedures (patient_id, doctor_id, procedure_name, procedure_date, status)
VALUES (4, 4, 'C-Section', '2025-03-04', 'Completed');
INSERT INTO Surgical_Procedures (patient_id, doctor_id, procedure_name, procedure_date, status)
VALUES (5, 5, 'Tonsillectomy', '2025-03-05', 'Completed');
INSERT INTO Surgical_Procedures (patient_id, doctor_id, procedure_name, procedure_date, status)
VALUES (6, 6, 'Hernia Repair', '2025-03-06', 'Scheduled');
INSERT INTO Surgical_Procedures (patient_id, doctor_id, procedure_name, procedure_date, status)
VALUES (7, 7, 'Gallbladder Removal', '2025-03-07', 'Scheduled');

CREATE TABLE Visitors (
    visitor_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    visitor_name VARCHAR(100) NOT NULL,
    visit_date DATE NOT NULL,
    relationship VARCHAR(50) NOT NULL,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
);

INSERT INTO Visitors (patient_id, visitor_name, visit_date, relationship)
VALUES (1, 'Linda Doe', '2025-02-18', 'Spouse'),
(2, 'Peter Smith', '2025-02-19', 'Brother'),
(3, 'Jane Johnson', '2025-02-20', 'Mother'),
(4, 'Rachel Brown', '2025-02-21', 'Friend'),
(5, 'David Davis', '2025-02-22', 'Father'),
(6, 'Megan Wilson', '2025-02-23', 'Sister'),
(7, 'Daniel Lee', '2025-02-24', 'Uncle');

CREATE TABLE Staff (
    staff_id INT AUTO_INCREMENT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    role VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone_number VARCHAR(20) NOT NULL
);

INSERT INTO Staff (first_name, last_name, role, email, phone_number)
VALUES 
    ('Sarah', 'Harris', 'Doctor', 'sarahharris@hospital.com', '7654321098'),
    ('John', 'Taylor', 'Radiologist', 'johntaylor@hospital.com', '8765432109'),
    ('Susan', 'Williams', 'Receptionist', 'susanwilliams@hospital.com', '9876543210'),
    ('James', 'Miller', 'Lab Technician', 'jamesmiller@hospital.com', '6543210987'),
    ('Patricia', 'Wilson', 'Cleaner', 'patriciawilson@hospital.com', '5432109876'),
    ('Robert', 'Brown', 'Administrator', 'robertbrown@hospital.com', '4321098765'),
    ('Jessica', 'Moore', 'Nurse', 'jessicamoore@hospital.com', '3210987654');

CREATE TABLE Staff_Training (
    training_id INT AUTO_INCREMENT PRIMARY KEY,
    staff_id INT,
    training_name VARCHAR(100) NOT NULL,
    training_date DATE NOT NULL,
    trainer_name VARCHAR(100) NOT NULL,
    FOREIGN KEY (staff_id) REFERENCES Staff(staff_id)
);

INSERT INTO Staff_Training (staff_id, training_name, training_date, trainer_name)
VALUES
    (1, 'Basic Life Support', '2025-01-10', 'Dr. Sarah Harris'),
    (2, 'Radiology Safety', '2025-02-01', 'Dr. John Taylor'),
    (3, 'Reception Management', '2025-03-15', 'Susan Williams'),
    (4, 'Laboratory Safety', '2025-02-20', 'James Miller'),
    (5, 'Sanitation and Cleanliness', '2025-01-30', 'Patricia Wilson'),
    (6, 'Administrative Procedures', '2025-02-10', 'Robert Brown'),
    (7, 'Nursing Fundamentals', '2025-03-05', 'Jessica Moore');

CREATE TABLE Healthcare_Plans (
    plan_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    plan_name VARCHAR(100) NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    status VARCHAR(50) NOT NULL,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
);

INSERT INTO Healthcare_Plans (patient_id, plan_name, start_date, end_date, status)
VALUES (1, 'Cardiac Rehabilitation Plan', '2025-01-20', '2025-07-20', 'Active'),
(2, 'Asthma Management', '2025-02-05', '2025-08-05', 'Active'),
(3, 'Diabetes Management Plan', '2025-02-10', '2025-08-10', 'Active'),
(4, 'Post-Surgical Recovery', '2025-01-28', '2025-03-28', 'Completed'),
(5, 'Anxiety Disorder Therapy', '2025-02-12', '2025-08-12', 'Active'),
(6, 'Migraine Management', '2025-02-15', '2025-08-15', 'Active'),
(7, 'Arthritis Pain Management', '2025-02-20', '2025-08-20', 'Active');

CREATE TABLE Referrals (
    referral_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    referred_doctor_id INT,
    referral_date DATE NOT NULL,
    reason_for_referral TEXT NOT NULL,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id),
    FOREIGN KEY (referred_doctor_id) REFERENCES Doctors(doctor_id)
);

INSERT INTO Referrals (patient_id, referred_doctor_id, referral_date, reason_for_referral)
VALUES (1, 2, '2025-01-15', 'Referred for knee surgery due to complications'),
(2, 3, '2025-02-05', 'Referred for further asthma management'),
(3, 4, '2025-02-12', 'Referred for mental health counseling'),
(4, 5, '2025-01-28', 'Referred for post-operative care'),
(5, 6, '2025-02-15', 'Referred for specialized migraine treatment'),
(6, 7, '2025-02-20', 'Referred for arthritis pain management'),
 (7, 1, '2025-02-25', 'Referred for heart health consultation');

CREATE TABLE Ambulance (
    ambulance_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    ambulance_date DATE NOT NULL,
    pickup_location TEXT NOT NULL,
    destination VARCHAR(100) NOT NULL,
    driver_name VARCHAR(50) NOT NULL,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
);

INSERT INTO Ambulance (patient_id, ambulance_date, pickup_location, destination, driver_name)
VALUES (1, '2025-02-01', '123 Main St', 'General Hospital', 'John Smith'),
(2, '2025-02-03', '456 Oak St', 'City Medical Center', 'Linda Green'),
(3, '2025-02-05', '789 Pine St', 'Community Health Center', 'Michael Brown'),
(4, '2025-02-07', '101 Maple St', 'University Hospital', 'Robert Taylor'),
(5, '2025-02-10', '202 Birch St', 'City Medical Center', 'David Lee'),
(6, '2025-02-15', '303 Cedar St', 'Community Health Center', 'Jennifer Davis'),
(7, '2025-02-18', '404 Elm St', 'General Hospital', 'Thomas Wilson');


CREATE TABLE Inventory (
    item_id INT AUTO_INCREMENT PRIMARY KEY,
    item_name VARCHAR(100) NOT NULL,
    quantity INT NOT NULL,
    unit_price DECIMAL(10, 2) NOT NULL,
    supplier_name VARCHAR(100) NOT NULL
);

INSERT INTO Inventory (item_name, quantity, unit_price, supplier_name)
VALUES ('Surgical Mask', 1000, 0.50, 'Health Supply Co.'),
('Gloves', 500, 0.25, 'Medical Gear Inc.'),
('Bandages', 300, 1.00, 'Health Supply Co.'),
('Syringes', 1000, 0.30, 'MedTech Ltd.'),
('Thermometers', 50, 15.00, 'BioMed Supplies'),
('Blood Pressure Monitors', 25, 50.00, 'Health Gear'),
('Wheelchairs', 20, 100.00, 'Care Supplies Inc.');


CREATE TABLE Emergency_Contacts (
    contact_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    contact_name VARCHAR(100) NOT NULL,
    relationship VARCHAR(50) NOT NULL,
    contact_phone VARCHAR(20) NOT NULL,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
);

INSERT INTO Emergency_Contacts (patient_id, contact_name, relationship, contact_phone)
VALUES (1, 'Jane Doe', 'Wife', '5555555555'),
(2, 'John Smith', 'Brother', '5556666666'),
(3, 'Mary Johnson', 'Mother', '5557777777'),
(4, 'Alex Brown', 'Father', '5558888888'),
(5, 'Linda Davis', 'Sister', '5559999999'),
(6, 'Michael Lee', 'Friend', '5550000000'),
(7, 'Karen Wilson', 'Aunt', '5551111111');


CREATE TABLE Medical_Equipment (
    equipment_id INT AUTO_INCREMENT PRIMARY KEY,
    equipment_name VARCHAR(100) NOT NULL,
    quantity INT NOT NULL,
    purchase_date DATE NOT NULL,
    status VARCHAR(50) NOT NULL
);

INSERT INTO Medical_Equipment (equipment_name, quantity, purchase_date, status)
VALUES ('ECG Machine', 5, '2025-01-01', 'Operational'),
('X-ray Machine', 3, '2025-01-15', 'Under Maintenance'),
('MRI Scanner', 2, '2025-02-01', 'Operational'),
('Ultrasound Machine', 4, '2025-03-01', 'Operational'),
('Defibrillator', 10, '2025-02-10', 'Operational'),
('Ventilator', 15, '2025-03-01', 'Operational');

CREATE TABLE Clinical_Trials (
    trial_id INT AUTO_INCREMENT PRIMARY KEY,
    trial_name VARCHAR(100) NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    status VARCHAR(50) NOT NULL
);

INSERT INTO Clinical_Trials (trial_name, start_date, end_date, status)
VALUES ('COVID-19 Vaccine Trial', '2025-01-01', '2025-12-31', 'Ongoing'),
('Cancer Treatment Trial', '2025-02-01', '2025-12-31', 'Ongoing'),
('Diabetes Drug Trial', '2025-03-01', '2025-12-31', 'Ongoing'),
('Heart Disease Treatment Trial', '2025-02-15', '2025-12-15', 'Ongoing'),
('Alzheimer\'s Drug Trial', '2025-01-20', '2025-12-20', 'Ongoing'),
('Asthma Medication Trial', '2025-03-01', '2025-09-01', 'Ongoing'),
('Flu Vaccine Trial', '2025-04-01', '2025-10-01', 'Ongoing');

CREATE TABLE Discharges (
    discharge_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    discharge_date DATE NOT NULL,
    reason_for_discharge TEXT NOT NULL,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
);

INSERT INTO Discharges (patient_id, discharge_date, reason_for_discharge)
VALUES (1, '2025-02-01', 'Stable condition after surgery'),
(2, '2025-02-05', 'Recovered from asthma attack'),
(3, '2025-02-10', 'Stable blood sugar levels'),
(4, '2025-01-30', 'Fully recovered from flu'),
(5, '2025-02-12', 'Mental health stabilized'),
(6, '2025-02-15', 'Pain under control'),
(7, '2025-02-20', 'Improved joint mobility');

CREATE TABLE Billing (
    bill_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    total_amount DECIMAL(10, 2) NOT NULL,
    bill_date DATE NOT NULL,
    payment_status VARCHAR(50) NOT NULL,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
);

INSERT INTO Billing (patient_id, total_amount, bill_date, payment_status)
VALUES (1, 5000.00, '2025-02-01', 'Paid'),
(2, 2000.00, '2025-02-05', 'Unpaid'),
(3, 1500.00, '2025-02-10', 'Paid'),
(4, 4000.00, '2025-01-30', 'Unpaid'),
(5, 3000.00, '2025-02-12', 'Paid'),
(6, 3500.00, '2025-02-15', 'Unpaid'),
7, 2500.00, '2025-02-20', 'Paid');

CREATE TABLE Lab_Reports (
    report_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    report_type VARCHAR(100) NOT NULL,
    report_date DATE NOT NULL,
    report_status VARCHAR(50) NOT NULL,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
);

INSERT INTO Lab_Reports (patient_id, report_type, report_date, report_status)
VALUES (1, 'Blood Test', '2025-01-15', 'Completed'),
(2, 'X-Ray', '2025-01-20', 'Pending'),
(3, 'MRI', '2025-01-25', 'Completed'),
(4, 'CT Scan', '2025-01-30', 'Completed'),
(5, 'Urine Test', '2025-02-01', 'Pending'),
(6, 'Blood Sugar Test', '2025-02-05', 'Completed'),
(7, 'Electrocardiogram (ECG)', '2025-02-10', 'Completed');

CREATE TABLE Patient_Emergency_Contacts (
    contact_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    emergency_contact_name VARCHAR(100) NOT NULL,
    relationship VARCHAR(50) NOT NULL,
    phone_number VARCHAR(20) NOT NULL,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
);

INSERT INTO Patient_Emergency_Contacts (patient_id, emergency_contact_name, relationship, phone_number)
VALUES (1, 'Emma Doe', 'Sister', '5551234567'),
(2, 'James Smith', 'Father', '5552345678'),
(3, 'Olivia Johnson', 'Friend', '5553456789'),
(4, 'Daniel Brown', 'Brother', '5554567890'),
(5, 'David Davis', 'Cousin', '5555678901'),
(6, 'Sarah Lee', 'Mother', '5556789012'),
(7, 'Michael Wilson', 'Father', '5557890123');

CREATE TABLE Admission_History (
    admission_id INT AUTO_INCREMENT PRIMARY KEY,
    patient_id INT,
    admission_date DATE NOT NULL,
    discharge_date DATE,
    reason_for_admission TEXT NOT NULL,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
);

INSERT INTO Admission_History (patient_id, admission_date, discharge_date, reason_for_admission)
VALUES (1, '2025-01-10', '2025-01-15', 'Heart Surgery'),
(2, '2025-01-15', '2025-01-20', 'Asthma Attack'),
(3, '2025-01-20', '2025-01-25', 'Surgical Recovery'),
(4, '2025-01-25', '2025-01-30', 'Flu'),
(5, '2025-02-01', '2025-02-05', 'Mental Health Crisis'),
(6, '2025-02-05', '2025-02-10', 'Arthritis Flare-up'),
(7, '2025-02-10', '2025-02-15', 'Post-surgical recovery'),


1. Get All Patients
SELECT * FROM Patients;
2. Get All Doctors
SELECT * FROM Doctors;
3. Get All Appointments
SELECT * FROM Appointments;
4. Get All Staff Members
SELECT * FROM Staff;
5. Get All Treatments
SELECT * FROM Treatments;
6. Get All Hospital Rooms
SELECT * FROM Hospital_Rooms;
7. Get All Patient Visits
SELECT * FROM Patient_Visits;
8. Get All Medications
SELECT * FROM Medications;
9. Get All Payments
SELECT * FROM Payments;
10. Get All Medical Records
SELECT * FROM Medical_Records;
11. Get All Prescriptions
SELECT * FROM Prescriptions;
12. Get All Insurance Providers
SELECT * FROM Insurance;
13. Get All Billing Information
SELECT * FROM Billing;
14. Get All Lab Reports
SELECT * FROM Lab_Reports;
15. Get All Staff Training Records
SELECT * FROM Staff_Training;
16. Get All Healthcare Plans
SELECT * FROM Healthcare_Plans;
17. Get All Referrals
SELECT * FROM Referrals;
18. Get All Ambulance Records
SELECT * FROM Ambulance;
19. Get All Inventory Items
SELECT * FROM Inventory;
20. Get All Emergency Contacts
SELECT * FROM Emergency_Contacts;
21. Get All Medical Equipment
SELECT * FROM Medical_Equipment;
22. Get All Clinical Trials
SELECT * FROM Clinical_Trials;
23. Get All Discharges
SELECT * FROM Discharges;
24. Get All Admission History
SELECT * FROM Admission_History;
25. Get All Billing Details
SELECT * FROM Billing;
26. Get All Lab Reports for a Specific Patient
SELECT * FROM Lab_Reports WHERE patient_id = 1;
27. Get All Patient Emergency Contacts
SELECT * FROM Patient_Emergency_Contacts;
28. Get All Prescription Records for a Specific Patient
SELECT * FROM Prescriptions WHERE patient_id = 1;
29. Get All Insurance Information for a Patient
SELECT * FROM Insurance WHERE patient_id = 1;
30. Get All Appointment Details for a Specific Doctor
SELECT * FROM Appointments WHERE doctor_id = 1;
Bonus: Get All Staff Members Working in a Specific Department
SELECT * FROM Staff WHERE department_id = 2;

