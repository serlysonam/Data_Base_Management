# Data_Base_Management
End to end Project (Using Oracle Database, fully design, implement, and document a database “backend” solution, as per specifications posted in the insulation unlimited case.

-- You can see a Demo of my Working Oracle here:
https://youtu.be/iYCD8FwwiQ4

-- You can see a Demo of my Working user interface here:
https://www.youtube.com/watch?v=_k--6SXZd_M![image](https://github.com/serlysonam/Data_Base_Management/assets/47883763/a71873d3-b9e0-41d9-8544-360491b2da67)


-- A final version of the Business Rules and ERD for the case.

Business Rules:

•	A Customer can receive zero or many proposals.
•	A proposal can be sent to one and only one customer.

•	A proposal can have zero or many work orders.
•	A work order can have one and only one proposal.

•	A work order can have one and many work assignment.
•	A work assignment can have maximum one work order.

•	A proposal can have zero or many invoice.
•	An invoice can have maximum one proposal.

•	A task can have zero or many proposals.
•	A proposal can have one to many tasks.

•	A work assignment can be assigned to one or many workers/labors assignment.
•	A labor assignment is assigned to maximum one work assignment.
 
•	A work assignment can have one or many materials assignment.
•	A material assignment can have zero to many work assignments.

•	A work assignment can have zero or many crew.
•	A crew can have zero to many work assignments.

•	A labor assignment can have maximum one employee.
•	An employee can have one or many labor assignments.

•	An employee might be in zero or one crew.
•	A crew can have one to many employees.

![image](https://github.com/serlysonam/Data_Base_Management/assets/47883763/539c6305-48fd-4a55-ab30-5cf729b66316)

SCHEMA:

•	customer (customer_no, customer_name, customer_address, customer_type)

•	task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours)

•	proposal (proposal_no, date_written, customer_no, est_method, status, decision_date, task_id)

•	work_order (work_order_no, proposal_no, work_date, notes, work_location_name, work_location_address, date_complete, manager_name, date_required)

•	material_assignment (material_id, material, unit_cost, qty_sent, field)

•	crew (crew_no, no_of_members, description)

•	employee (employee_id, name, role, crew_no)

•	labor_assignment (labor_assignment_id, employee_id, rate, hrs_est, hrs_used, authorization_date, authorized_by)

•	work_assignment (assignment_no, work_order_no, start_date, finish_date, location_name, location_address, supervisor, vehicle_no, material_id, crew_no, employee_id, lobor_assignment_id)

•	invoice (invoice_no, proposal_no, invoice_date, start_date, end_date, location, amount, date_complete, total_before_tax, tax, total_after_tax)

CREATING THE TABLES: 
CREATE TABLE customer(
    customer_no NUMBER PRIMARY KEY,
    customer_name VARCHAR2(50),
    customer_address VARCHAR2(100),
    customer_type VARCHAR2(50)
);

CREATE TABLE task(
    task_id VARCHAR2(50) PRIMARY KEY,
    task_description VARCHAR2(250),
    date_completion DATE,
    square_feet NUMBER,
    pricepersqft FLOAT,
    amount FLOAT,
    status VARCHAR2(50),
    est_hours NUMBER
);

CREATE TABLE proposal(
    proposal_no NUMBER PRIMARY KEY,
    date_written DATE,
    customer_no NUMBER,
    CONSTRAINT customer_no FOREIGN KEY (customer_no) REFERENCES customer(customer_no),
    est_method VARCHAR2(50),
    status VARCHAR2(50),
    decision_date DATE,
    task_id VARCHAR2(50),
    CONSTRAINT task_id FOREIGN KEY (task_id) REFERENCES task(task_id)
);

CREATE TABLE work_order(
    work_order_no NUMBER PRIMARY KEY,
    proposal_no NUMBER,
    FOREIGN KEY (proposal_no) REFERENCES proposal(proposal_no),
    work_date DATE,
    notes VARCHAR(50),
    work_location_name VARCHAR(50),
    work_location_address VARCHAR(50),
    date_complete DATE,
    manager_name VARCHAR2(50),
    date_required DATE
);

CREATE TABLE material_assignment(
    material_id VARCHAR2(20) PRIMARY KEY,
    material VARCHAR2(20),
    unit_cost FLOAT,
    qty_sent NUMBER,
    field VARCHAR2(20)
);

CREATE TABLE crew(
    crew_no NUMBER PRIMARY KEY,
    no_of_members NUMBER,
    description VARCHAR(100)
);

CREATE TABLE employee(
    employee_id VARCHAR2(20) PRIMARY KEY,
    name VARCHAR2(20),
    role VARCHAR2(20),
    crew_no NUMBER,
    FOREIGN KEY (crew_no) REFERENCES crew(crew_no)
);

CREATE TABLE labor_assignment(
    labor_assignment_id NUMBER PRIMARY KEY,
    employee_id VARCHAR2(20),
    FOREIGN KEY (employee_id) REFERENCES employee(employee_id),
    rate FLOAT,
    hrs_est NUMBER,
    hrs_used NUMBER,
    authorization_date DATE,
    authorized_by VARCHAR2(20)
);


CREATE TABLE work_assignment(
    assignment_no NUMBER PRIMARY KEY,
    work_order_no NUMBER,
    FOREIGN KEY (work_order_no) REFERENCES work_order(work_order_no),
    start_date DATE,
    finish_date DATE,
    location_name VARCHAR2(50),
    location_address VARCHAR2(50),
    supervisor VARCHAR2(50),
    vehicle_no VARCHAR2(50),
    material_id VARCHAR2(20),
    FOREIGN KEY (material_id) REFERENCES material_assignment(material_id),
    crew_no NUMBER,
    FOREIGN KEY (crew_no) REFERENCES crew(crew_no),
    labor_assignment_id NUMBER,
    FOREIGN KEY (labor_assignment_id) REFERENCES labor_assignment(labor_assignment_id)
);

CREATE TABLE invoice(
    invoice_no VARCHAR2(50) PRIMARY KEY,
    proposal_no NUMBER,
    FOREIGN KEY (proposal_no) REFERENCES proposal(proposal_no),
    invoice_date DATE,
    start_date DATE,
    end_date DATE,
    location VARCHAR2(50),
    amount FLOAT,
    date_complete DATE,
    total_befor_tax FLOAT,
    tax FLOAT,
    total_after_tax FLOAT
);

INSERTING DATASETS:

For customer table:

INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (1, 'Thomas Group','111 Oak Lane, Mountainside', 'Corporate');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (2, 'Lewis', '888 Maple Street, Seaside', 'Corporate');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (3, 'Smith', '789 Oak St, City, Country', 'Individual');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (4, 'Johnson Enterprises', '101 Pine St, City, Country', 'Corporate');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (5, 'Mary Johnson', '321 Maple St, City, Country', 'Individual');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (6, 'ABC Plumbing', '555 Cedar St, City, Country', 'Corporate');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (7, 'ACME Construction', '777 Walnut St, City, Country', 'Corporate');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (8, 'John Doe', '999 Birch St, City, Country', 'Individual');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (9, 'Jane Smith', '888 Spruce St, City, Country', 'Individual');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (10, 'Smith', '222 Pine St, City, Country', 'Corporate');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (11, 'Miller Company', '789 Pine Road, Townburg', 'Corporate');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (12, 'Brown Corporation', '101 Elm Lane, Villagetown', 'Corporate');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (13, 'Wilson Industries', '321 Cedar Street, Hamlet City', 'Corporate');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (14, 'Taylor', '555 Walnut Avenue, Countryside', 'Corporate');
INSERT INTO customer (customer_no, customer_name, customer_address, customer_type) VALUES (15, 'Clark Enterprises', '777 Birch Road, Lakeside', 'Corporate');


For task table:

-- Batch 1
INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1001, 'Install new flooring in office area', TO_DATE('2024-03-20', 'YYYY-MM-DD'), 500, 3.5, NULL, 'In Progress', 10);

INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1002, 'Paint exterior walls of building', TO_DATE('2024-03-25', 'YYYY-MM-DD'), 700, 2.8, NULL, 'Scheduled', 15);

INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1003, 'Repair plumbing in restroom', TO_DATE('2024-03-18', 'YYYY-MM-DD'), 300, 4.2, NULL, 'Pending', 8);

-- Batch 2
INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1004, 'Replace roof shingles', TO_DATE('2024-03-30', 'YYYY-MM-DD'), 800, 5.0, NULL, 'In Progress', 20);

INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1005, 'Install new lighting fixtures', TO_DATE('2024-03-22', 'YYYY-MM-DD'), 400, 6.0, NULL, 'Completed', 10);

INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1006, 'Renovate conference room', TO_DATE('2024-04-05', 'YYYY-MM-DD'), 600, 4.9, NULL, 'In Progress', 12);

-- Batch 3
INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1007, 'Clean and maintain HVAC system', TO_DATE('2024-03-28', 'YYYY-MM-DD'), 900, 3.0, NULL, 'Scheduled', 25);

INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1008, 'Construct additional parking space', TO_DATE('2024-04-10', 'YYYY-MM-DD'), 1000, 7.5, NULL, 'In Progress', 30);

INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1009, 'Install security cameras', TO_DATE('2024-03-24', 'YYYY-MM-DD'), 200, 4.5, NULL, 'Completed', 5);

-- Batch 4
INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1010, 'Upgrade server infrastructure', TO_DATE('2024-04-15', 'YYYY-MM-DD'), 1200, 8.2, NULL, 'Scheduled', 35);

INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1011, 'Repair cracked pavement', TO_DATE('2024-03-26', 'YYYY-MM-DD'), 600, 3.2, NULL, 'Pending', 10);

INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1012, 'Paint interior walls of lobby', TO_DATE('2024-03-21', 'YYYY-MM-DD'), 800, 2.7, NULL, 'Completed', 20);

-- Batch 5
INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1013, 'Install new windows', TO_DATE('2024-03-29', 'YYYY-MM-DD'), 400, 9.0, NULL, 'In Progress', 15);

INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1014, 'Upgrade network infrastructure', TO_DATE('2024-04-20', 'YYYY-MM-DD'), 1000, 6.5, NULL, 'Scheduled', 25);

INSERT INTO task (task_id, task_description, date_completion, square_feet, pricepersqft, amount, status, est_hours) 
VALUES 
(1015, 'Replace office furniture', TO_DATE('2024-04-25', 'YYYY-MM-DD'), 700, 3.8, NULL, 'Pending', 20);


For proposal table:

INSERT INTO proposal (proposal_no, date_written, customer_no, est_method, status, decision_date, task_id) 
VALUES 
(1, TO_DATE('2024-03-01', 'YYYY-MM-DD'), 1, 'Standard', 'Pending', NULL, 1001);
INSERT INTO proposal (proposal_no, date_written, customer_no, est_method, status, decision_date, task_id) 
VALUES
(2, TO_DATE('2024-03-05', 'YYYY-MM-DD'), 2, 'Advanced', 'Approved', TO_DATE('2024-03-10', 'YYYY-MM-DD'), 1002);
INSERT INTO proposal (proposal_no, date_written, customer_no, est_method, status, decision_date, task_id) 
VALUES
(3, TO_DATE('2024-03-10', 'YYYY-MM-DD'), 3, 'Basic', 'Pending', NULL, 1003);
INSERT INTO proposal (proposal_no, date_written, customer_no, est_method, status, decision_date, task_id) 
VALUES
(4, TO_DATE('2024-03-15', 'YYYY-MM-DD'), 4, 'Standard', 'Rejected', TO_DATE('2024-03-20', 'YYYY-MM-DD'), 1004);
INSERT INTO proposal (proposal_no, date_written, customer_no, est_method, status, decision_date, task_id) 
VALUES
(5, TO_DATE('2024-03-20', 'YYYY-MM-DD'), 5, 'Advanced', 'Approved', TO_DATE('2024-03-25', 'YYYY-MM-DD'), 1005);
INSERT INTO proposal (proposal_no, date_written, customer_no, est_method, status, decision_date, task_id) 
VALUES
(6, TO_DATE('2024-03-25', 'YYYY-MM-DD'), 1, 'Standard', 'Pending', NULL, 1006);
INSERT INTO proposal (proposal_no, date_written, customer_no, est_method, status, decision_date, task_id) 
VALUES
(7, TO_DATE('2024-03-30', 'YYYY-MM-DD'), 2, 'Advanced', 'Pending', NULL, 1007);
INSERT INTO proposal (proposal_no, date_written, customer_no, est_method, status, decision_date, task_id) 
VALUES
(8, TO_DATE('2024-04-01', 'YYYY-MM-DD'), 3, 'Basic', 'Approved', TO_DATE('2024-04-05', 'YYYY-MM-DD'), 1008);
INSERT INTO proposal (proposal_no, date_written, customer_no, est_method, status, decision_date, task_id) 
VALUES
(9, TO_DATE('2024-04-05', 'YYYY-MM-DD'), 4, 'Standard', 'Pending', NULL, 1009);
INSERT INTO proposal (proposal_no, date_written, customer_no, est_method, status, decision_date, task_id) 
VALUES
(10, TO_DATE('2024-04-10', 'YYYY-MM-DD'), 5, 'Advanced', 'Approved', TO_DATE('2024-04-15', 'YYYY-MM-DD'), 1010);


For work_order table:

INSERT INTO work_order (work_order_no, proposal_no, work_date, notes, work_location_name, work_location_address, date_complete, manager_name, date_required) 
VALUES 
(1, 1, TO_DATE('2024-03-05', 'YYYY-MM-DD'), 'Replace flooring in office area', 'Office Area', '123 Main Street', NULL, 'John Smith', TO_DATE('2024-03-15', 'YYYY-MM-DD'));
INSERT INTO work_order (work_order_no, proposal_no, work_date, notes, work_location_name, work_location_address, date_complete, manager_name, date_required) 
VALUES 
(2, 2, TO_DATE('2024-03-10', 'YYYY-MM-DD'), 'Paint exterior walls of building', 'Building Exterior', '456 Elm Street', NULL, 'Jane Doe', TO_DATE('2024-03-20', 'YYYY-MM-DD'));
INSERT INTO work_order (work_order_no, proposal_no, work_date, notes, work_location_name, work_location_address, date_complete, manager_name, date_required) 
VALUES
(3, 3, TO_DATE('2024-03-15', 'YYYY-MM-DD'), 'Repair plumbing in restroom', 'Restroom', '789 Oak Avenue', NULL, 'Michael Johnson', TO_DATE('2024-03-25', 'YYYY-MM-DD'));
INSERT INTO work_order (work_order_no, proposal_no, work_date, notes, work_location_name, work_location_address, date_complete, manager_name, date_required) 
VALUES
(4, 4, TO_DATE('2024-03-20', 'YYYY-MM-DD'), 'Replace roof shingles', 'Roof', '101 Pine Road', NULL, 'Emily Brown', TO_DATE('2024-03-30', 'YYYY-MM-DD'));
INSERT INTO work_order (work_order_no, proposal_no, work_date, notes, work_location_name, work_location_address, date_complete, manager_name, date_required) 
VALUES
(5, 5, TO_DATE('2024-03-25', 'YYYY-MM-DD'), 'Install new lighting fixtures', 'Office Area', '321 Cedar Lane', NULL, 'David Wilson', TO_DATE('2024-04-05', 'YYYY-MM-DD'));
INSERT INTO work_order (work_order_no, proposal_no, work_date, notes, work_location_name, work_location_address, date_complete, manager_name, date_required) 
VALUES
(6, 6, TO_DATE('2024-04-01', 'YYYY-MM-DD'), 'Renovate conference room', 'Conference Room', '555 Walnut Avenue', NULL, 'Jennifer Taylor', TO_DATE('2024-04-10', 'YYYY-MM-DD'));
INSERT INTO work_order (work_order_no, proposal_no, work_date, notes, work_location_name, work_location_address, date_complete, manager_name, date_required) 
VALUES
(7, 7, TO_DATE('2024-04-05', 'YYYY-MM-DD'), 'Clean and maintain HVAC system', 'HVAC System', '777 Birch Road', NULL, 'Christopher Clark', TO_DATE('2024-04-15', 'YYYY-MM-DD'));
INSERT INTO work_order (work_order_no, proposal_no, work_date, notes, work_location_name, work_location_address, date_complete, manager_name, date_required) 
VALUES
(8, 8, TO_DATE('2024-04-10', 'YYYY-MM-DD'), 'Construct additional parking space', 'Parking Area', '999 Pine Lane', NULL, 'Sarah Adams', TO_DATE('2024-04-20', 'YYYY-MM-DD'));
INSERT INTO work_order (work_order_no, proposal_no, work_date, notes, work_location_name, work_location_address, date_complete, manager_name, date_required) 
VALUES
(9, 9, TO_DATE('2024-04-15', 'YYYY-MM-DD'), 'Install security cameras', 'Security Area', '111 Oak Lane', NULL, 'Matthew Thomas', TO_DATE('2024-04-25', 'YYYY-MM-DD'));
INSERT INTO work_order (work_order_no, proposal_no, work_date, notes, work_location_name, work_location_address, date_complete, manager_name, date_required) 
VALUES
(10, 10, TO_DATE('2024-04-20', 'YYYY-MM-DD'), 'Upgrade server infrastructure', 'Server Room', '888 Maple Street', NULL, 'Olivia Lewis', TO_DATE('2024-04-30', 'YYYY-MM-DD'));

For material_assignment table:

INSERT INTO material_assignment (material_id, material, unit_cost, qty_sent, field) 
VALUES (1, 'Concrete', 50.00, 100, 'Construction');

INSERT INTO material_assignment (material_id, material, unit_cost, qty_sent, field) 
VALUES(2, 'Bricks', 0.25, 5000, 'Construction');

INSERT INTO material_assignment (material_id, material, unit_cost, qty_sent, field) 
VALUES(3, 'Paint', 20.00, 10, 'Painting');

INSERT INTO material_assignment (material_id, material, unit_cost, qty_sent, field) 
VALUES(4, 'Nails', 0.05, 2000, 'Construction');

INSERT INTO material_assignment (material_id, material, unit_cost, qty_sent, field) 
VALUES(5, 'Pipes', 10.00, 50, 'Plumbing');

INSERT INTO material_assignment (material_id, material, unit_cost, qty_sent, field) 
VALUES(6, 'Wires', 15.00, 100, 'Electrical');

INSERT INTO material_assignment (material_id, material, unit_cost, qty_sent, field) 
VALUES(7, 'Tiles', 2.50, 1000, 'Flooring');

INSERT INTO material_assignment (material_id, material, unit_cost, qty_sent, field) 
VALUES(8, 'Insulation', 30.00, 50, 'Construction');

INSERT INTO material_assignment (material_id, material, unit_cost, qty_sent, field) 
VALUES(9, 'Wood', 40.00, 200, 'Construction');

INSERT INTO material_assignment (material_id, material, unit_cost, qty_sent, field) 
VALUES(10, 'Drywall', 8.00, 500, 'Construction');


For crew table:

INSERT INTO crew (crew_no, no_of_members, description) VALUES (1, 5, 'Construction crew for building construction');

INSERT INTO crew (crew_no, no_of_members, description)VALUES(2, 3, 'Painting crew for interior painting');

INSERT INTO crew (crew_no, no_of_members, description) VALUES(3, 4, 'Plumbing crew for installing plumbing systems');

INSERT INTO crew (crew_no, no_of_members, description) VALUES(4, 6, 'Electrical crew for wiring and installation');

INSERT INTO crew (crew_no, no_of_members, description) VALUES(5, 8, 'Roofing crew for roof installation and repair');

INSERT INTO crew (crew_no, no_of_members, description) VALUES(6, 4, 'Flooring crew for installing floor materials');

INSERT INTO crew (crew_no, no_of_members, description) VALUES(7, 5, 'Landscaping crew for outdoor landscaping work');

INSERT INTO crew (crew_no, no_of_members, description) VALUES(8, 7, 'HVAC crew for heating, ventilation, and air conditioning installation');

INSERT INTO crew (crew_no, no_of_members, description) VALUES(9, 6, 'Demolition crew for tearing down structures');

INSERT INTO crew (crew_no, no_of_members, description) VALUES(10, 4, 'Maintenance crew for general maintenance tasks');

For employee table:

INSERT INTO employee (employee_id, name, role, crew_no) VALUES ('E001', 'John Doe', 'Foreman', 1);

INSERT INTO employee (employee_id, name, role, crew_no) VALUES('E002', 'Jane Smith', 'Laborer', 1);

INSERT INTO employee (employee_id, name, role, crew_no) VALUES('E003', 'David Johnson', 'Painter', 2);

INSERT INTO employee (employee_id, name, role, crew_no) VALUES('E004', 'Sarah Williams', 'Plumber', 3);

INSERT INTO employee (employee_id, name, role, crew_no) VALUES('E005', 'Michael Brown', 'Electrician', 4);

INSERT INTO employee (employee_id, name, role, crew_no) VALUES('E006', 'Jennifer Lee', 'Laborer', 1);

INSERT INTO employee (employee_id, name, role, crew_no) VALUES('E007', 'Christopher Davis', 'Carpenter', 1);

INSERT INTO employee (employee_id, name, role, crew_no) VALUES('E008', 'Amanda Martinez', 'Painter', 2);

INSERT INTO employee (employee_id, name, role, crew_no) VALUES('E009', 'James Wilson', 'Electrician', 4);

INSERT INTO employee (employee_id, name, role, crew_no) VALUES('E010', 'Emily Anderson', 'Laborer', 1);


For labor_assignment table:

INSERT INTO labor_assignment (labor_assignment_id, employee_id, rate, hrs_est, hrs_used, authorization_date, authorized_by) 
VALUES (1, 'E001', 25.00, 40, 35, TO_DATE('2024-03-05', 'YYYY-MM-DD'), 'Manager1');

INSERT INTO labor_assignment (labor_assignment_id, employee_id, rate, hrs_est, hrs_used, authorization_date, authorized_by) 
VALUE(2, 'E002', 20.00, 35, 30, TO_DATE('2024-03-06', 'YYYY-MM-DD'), 'Manager2');

INSERT INTO labor_assignment (labor_assignment_id, employee_id, rate, hrs_est, hrs_used, authorization_date, authorized_by) 
VALUES(3, 'E003', 30.00, 45, 40, TO_DATE('2024-03-07', 'YYYY-MM-DD'), 'Manager3');

INSERT INTO labor_assignment (labor_assignment_id, employee_id, rate, hrs_est, hrs_used, authorization_date, authorized_by) 
VALUES(4, 'E004', 18.00, 30, 25, TO_DATE('2024-03-08', 'YYYY-MM-DD'), 'Manager1');

INSERT INTO labor_assignment (labor_assignment_id, employee_id, rate, hrs_est, hrs_used, authorization_date, authorized_by) 
VALUES(5, 'E005', 22.00, 35, 30, TO_DATE('2024-03-09', 'YYYY-MM-DD'), 'Manager2');

INSERT INTO labor_assignment (labor_assignment_id, employee_id, rate, hrs_est, hrs_used, authorization_date, authorized_by) 
VALUES(6, 'E006', 28.00, 40, 35, TO_DATE('2024-03-10', 'YYYY-MM-DD'), 'Manager3');

INSERT INTO labor_assignment (labor_assignment_id, employee_id, rate, hrs_est, hrs_used, authorization_date, authorized_by) 
VALUES(7, 'E007', 18.00, 30, 25, TO_DATE('2024-03-11', 'YYYY-MM-DD'), 'Manager1');

INSERT INTO labor_assignment (labor_assignment_id, employee_id, rate, hrs_est, hrs_used, authorization_date, authorized_by) 
VALUES(8, 'E008', 25.00, 40, 35, TO_DATE('2024-03-12', 'YYYY-MM-DD'), 'Manager2');

INSERT INTO labor_assignment (labor_assignment_id, employee_id, rate, hrs_est, hrs_used, authorization_date, authorized_by) 
VALUES(9, 'E009', 20.00, 35, 30, TO_DATE('2024-03-13', 'YYYY-MM-DD'), 'Manager3');

INSERT INTO labor_assignment (labor_assignment_id, employee_id, rate, hrs_est, hrs_used, authorization_date, authorized_by) 
VALUES(10, 'E010', 30.00, 45, 40, TO_DATE('2024-03-14', 'YYYY-MM-DD'), 'Manager1');


For work_assignment table:

INSERT INTO work_assignment (assignment_no, work_order_no, start_date, finish_date, location_name, location_address, supervisor, vehicle_no, material_id, crew_no, employee_id, labor_assignment_id) 
VALUES 
(1, 1, TO_DATE('2024-03-05', 'YYYY-MM-DD'), TO_DATE('2024-03-10', 'YYYY-MM-DD'), 'Construction Site A', '123 Main Street', 'John Doe', 'V001', 1, 1, 'E001', 1);

INSERT INTO work_assignment (assignment_no, work_order_no, start_date, finish_date, location_name, location_address, supervisor, vehicle_no, material_id, crew_no, employee_id, labor_assignment_id) 
VALUES
(2, 2, TO_DATE('2024-03-10', 'YYYY-MM-DD'), TO_DATE('2024-03-15', 'YYYY-MM-DD'), 'Building Exterior', '456 Elm Street', 'Jane Smith', 'V002', 2, 2, 'E002', 2);

INSERT INTO work_assignment (assignment_no, work_order_no, start_date, finish_date, location_name, location_address, supervisor, vehicle_no, material_id, crew_no, employee_id, labor_assignment_id) 
VALUES
(3, 3, TO_DATE('2024-03-15', 'YYYY-MM-DD'), TO_DATE('2024-03-20', 'YYYY-MM-DD'), 'Restroom Area', '789 Oak Avenue', 'David Johnson', 'V003', 3, 3, 'E003', 3);

INSERT INTO work_assignment (assignment_no, work_order_no, start_date, finish_date, location_name, location_address, supervisor, vehicle_no, material_id, crew_no, employee_id, labor_assignment_id) 
VALUES
(4, 4, TO_DATE('2024-03-20', 'YYYY-MM-DD'), TO_DATE('2024-03-25', 'YYYY-MM-DD'), 'Roofing Site', '101 Pine Road', 'Sarah Williams', 'V004', 4, 4, 'E004', 4);

INSERT INTO work_assignment (assignment_no, work_order_no, start_date, finish_date, location_name, location_address, supervisor, vehicle_no, material_id, crew_no, employee_id, labor_assignment_id) 
VALUES
(5, 5, TO_DATE('2024-03-25', 'YYYY-MM-DD'), TO_DATE('2024-03-30', 'YYYY-MM-DD'), 'Office Area', '321 Cedar Lane', 'Michael Brown', 'V005', 5, 1, 'E005', 5);

INSERT INTO work_assignment (assignment_no, work_order_no, start_date, finish_date, location_name, location_address, supervisor, vehicle_no, material_id, crew_no, employee_id, labor_assignment_id) 
VALUES
(6, 6, TO_DATE('2024-03-30', 'YYYY-MM-DD'), TO_DATE('2024-04-05', 'YYYY-MM-DD'), 'Conference Room', '555 Walnut Avenue', 'Jennifer Lee', 'V006', 6, 2, 'E006', 6);

INSERT INTO work_assignment (assignment_no, work_order_no, start_date, finish_date, location_name, location_address, supervisor, vehicle_no, material_id, crew_no, employee_id, labor_assignment_id) 
VALUES
(7, 7, TO_DATE('2024-04-05', 'YYYY-MM-DD'), TO_DATE('2024-04-10', 'YYYY-MM-DD'), 'HVAC System', '777 Birch Road', 'Christopher Davis', 'V007', 7, 3, 'E007', 7);

INSERT INTO work_assignment (assignment_no, work_order_no, start_date, finish_date, location_name, location_address, supervisor, vehicle_no, material_id, crew_no, employee_id, labor_assignment_id) 
VALUES
(8, 8, TO_DATE('2024-04-10', 'YYYY-MM-DD'), TO_DATE('2024-04-15', 'YYYY-MM-DD'), 'Parking Area', '999 Pine Lane', 'Amanda Martinez', 'V008', 8, 4, 'E008', 8);

INSERT INTO work_assignment (assignment_no, work_order_no, start_date, finish_date, location_name, location_address, supervisor, vehicle_no, material_id, crew_no, employee_id, labor_assignment_id) 
VALUES
(9, 9, TO_DATE('2024-04-15', 'YYYY-MM-DD'), TO_DATE('2024-04-20', 'YYYY-MM-DD'), 'Security Area', '111 Oak Lane', 'James Wilson', 'V009', 9, 5, 'E009', 9);

INSERT INTO work_assignment (assignment_no, work_order_no, start_date, finish_date, location_name, location_address, supervisor, vehicle_no, material_id, crew_no, employee_id, labor_assignment_id) 
VALUES
(10, 10, TO_DATE('2024-04-20', 'YYYY-MM-DD'), TO_DATE('2024-04-25', 'YYYY-MM-DD'), 'Server Room', '888 Maple Street', 'Emily Anderson', 'V010', 10, 1, 'E010', 10);


For invoice table:

INSERT INTO invoice (invoice_no, proposal_no, invoice_date, start_date, end_date, location, amount, date_complete, total_before_tax, tax, total_after_tax) 
VALUES 
('INV001', 1, TO_DATE('2024-03-10', 'YYYY-MM-DD'), TO_DATE('2024-03-01', 'YYYY-MM-DD'), TO_DATE('2024-03-15', 'YYYY-MM-DD'), 'Construction Site A', 5000.00, TO_DATE('2024-03-15', 'YYYY-MM-DD'), 4500.00, 450.00, 4950.00);

INSERT INTO invoice (invoice_no, proposal_no, invoice_date, start_date, end_date, location, amount, date_complete, total_before_tax, tax, total_after_tax) 
VALUES 
('INV002', 2, TO_DATE('2024-03-15', 'YYYY-MM-DD'), TO_DATE('2024-03-10', 'YYYY-MM-DD'), TO_DATE('2024-03-20', 'YYYY-MM-DD'), 'Building Exterior', 3000.00, TO_DATE('2024-03-20', 'YYYY-MM-DD'), 2700.00, 270.00, 2970.00);

INSERT INTO invoice (invoice_no, proposal_no, invoice_date, start_date, end_date, location, amount, date_complete, total_before_tax, tax, total_after_tax) 
VALUES 
('INV003', 3, TO_DATE('2024-03-20', 'YYYY-MM-DD'), TO_DATE('2024-03-15', 'YYYY-MM-DD'), TO_DATE('2024-03-25', 'YYYY-MM-DD'), 'Restroom Area', 4000.00, TO_DATE('2024-03-25', 'YYYY-MM-DD'), 3600.00, 360.00, 3960.00);

INSERT INTO invoice (invoice_no, proposal_no, invoice_date, start_date, end_date, location, amount, date_complete, total_before_tax, tax, total_after_tax) 
VALUES 
('INV004', 4, TO_DATE('2024-03-25', 'YYYY-MM-DD'), TO_DATE('2024-03-20', 'YYYY-MM-DD'), TO_DATE('2024-03-30', 'YYYY-MM-DD'), 'Roofing Site', 6000.00, TO_DATE('2024-03-30', 'YYYY-MM-DD'), 5400.00, 540.00, 5940.00);

INSERT INTO invoice (invoice_no, proposal_no, invoice_date, start_date, end_date, location, amount, date_complete, total_before_tax, tax, total_after_tax) 
VALUES 
('INV005', 5, TO_DATE('2024-03-30', 'YYYY-MM-DD'), TO_DATE('2024-03-25', 'YYYY-MM-DD'), TO_DATE('2024-04-05', 'YYYY-MM-DD'), 'Office Area', 5500.00, TO_DATE('2024-04-05', 'YYYY-MM-DD'), 4950.00, 495.00, 5445.00);

INSERT INTO invoice (invoice_no, proposal_no, invoice_date, start_date, end_date, location, amount, date_complete, total_before_tax, tax, total_after_tax) 
VALUES 
('INV006', 6, TO_DATE('2024-04-05', 'YYYY-MM-DD'), TO_DATE('2024-03-30', 'YYYY-MM-DD'), TO_DATE('2024-04-10', 'YYYY-MM-DD'), 'Conference Room', 3500.00, TO_DATE('2024-04-10', 'YYYY-MM-DD'), 3150.00, 315.00, 3465.00);

INSERT INTO invoice (invoice_no, proposal_no, invoice_date, start_date, end_date, location, amount, date_complete, total_before_tax, tax, total_after_tax) 
VALUES 
('INV007', 7, TO_DATE('2024-04-10', 'YYYY-MM-DD'), TO_DATE('2024-04-05', 'YYYY-MM-DD'), TO_DATE('2024-04-15', 'YYYY-MM-DD'), 'HVAC System', 4800.00, TO_DATE('2024-04-15', 'YYYY-MM-DD'), 4320.00, 432.00, 4752.00);

INSERT INTO invoice (invoice_no, proposal_no, invoice_date, start_date, end_date, location, amount, date_complete, total_before_tax, tax, total_after_tax) 
VALUES 
('INV008', 8, TO_DATE('2024-04-15', 'YYYY-MM-DD'), TO_DATE('2024-04-10', 'YYYY-MM-DD'), TO_DATE('2024-04-20', 'YYYY-MM-DD'), 'Parking Area', 5100.00, TO_DATE('2024-04-20', 'YYYY-MM-DD'), 4590.00, 459.00, 5049.00);

INSERT INTO invoice (invoice_no, proposal_no, invoice_date, start_date, end_date, location, amount, date_complete, total_before_tax, tax, total_after_tax) 
VALUES 
('INV009', 9, TO_DATE('2024-04-20', 'YYYY-MM-DD'), TO_DATE('2024-04-15', 'YYYY-MM-DD'), TO_DATE('2024-04-25', 'YYYY-MM-DD'), 'Security Area', 4300.00, TO_DATE('2024-04-25', 'YYYY-MM-DD'), 3870.00, 387.00, 4263.00);

INSERT INTO invoice (invoice_no, proposal_no, invoice_date, start_date, end_date, location, amount, date_complete, total_before_tax, tax, total_after_tax) 
VALUES 
('INV010', 10, TO_DATE('2024-04-25', 'YYYY-MM-DD'), TO_DATE('2024-04-20', 'YYYY-MM-DD'), TO_DATE('2024-04-30', 'YYYY-MM-DD'), 'Server Room', 6000.00, TO_DATE('2024-04-30', 'YYYY-MM-DD'), 5400.00, 540.00, 5940.00);

Technical Manual (report) for DB backend: 
Single table query:
SELECT * FROM customer;
2- table query:
SELECT customer_name, customer_type
FROM customer c
LEFT JOIN proposal p 
ON c.customer_no = p.customer_no;

SELECT task_description, amount,est_hours
FROM task t
FULL OUTER JOIN proposal p 
ON t.task_id = p.task_id;

SELECT material_id, start_date, finish_date
FROM work_assignment ma
FULL OUTER JOIN work_assignment wa
ON ma.material_id = wa.material_id;

3- table query:
SELECT wa.start_date, wa.finish_date, e.employee_id, wa.location_name,cr.crew_no
FROM work_assignment wa
JOIN labor_assignment la
ON la.labor_assignment_id = wa.labor_assignment_id
JOIN employee e
ON e.employee_id = la.employee_id
JOIN crew cr
ON cr.crew_no = e.crew_no;

-- (Using Oracle Apex, design and develop a “front-end interface” (i.e., Forms and Reports), that meets all the requirements from the “inherited” project case.
I have inherited the Financial Loan Case and developed the front-end interface with oracle Apex.

-- You can see a Demo of my Working interface here:
https://www.youtube.com/watch?v=_k--6SXZd_M![image](https://github.com/serlysonam/Data_Base_Management/assets/47883763/a71873d3-b9e0-41d9-8544-360491b2da67)

