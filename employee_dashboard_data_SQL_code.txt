CREATE TABLE employee_dashboard_data (
    employee_id VARCHAR(10) PRIMARY KEY,
    employee_name VARCHAR(100) NOT NULL UNIQUE,
    employee_contact VARCHAR(15) UNIQUE,
    employee_location VARCHAR(50),
    age INT,
    gender VARCHAR(10),
    employee_position VARCHAR(100),
    employee_branch VARCHAR(50),
    reporting_employer_id VARCHAR(10),
    reporting_employer_name VARCHAR(100),
    reporting_employer_position VARCHAR(100),
    hire_date DATE,
    termination_date DATE, -- NULL if employee is active
    is_active BOOLEAN,
    days_present_last_year INT,
    days_absent_last_year INT,
    total_working_days_last_year INT,
    attendance_percentage DECIMAL(5, 2), -- Calculated field
    reason_for_attrition VARCHAR(255), -- Populated if employee is terminated
    monthly_salary DECIMAL(10, 2),
    years_of_service INT -- Calculated field
);

-- Insert 1000 rows of sample data
INSERT INTO employee_dashboard_data (
    employee_id, employee_name, employee_contact, employee_location, age, gender,
    employee_position, employee_branch, reporting_employer_id, reporting_employer_name,
    reporting_employer_position, hire_date, termination_date, is_active,
    days_present_last_year, days_absent_last_year, total_working_days_last_year,
    attendance_percentage, reason_for_attrition, monthly_salary, years_of_service
)
SELECT
    -- employee_id (e.g., EMP0001)
    'EMP' || LPAD(s.id::TEXT, 4, '0'),
    -- employee_name (unique, short)
    (ARRAY['Alice', 'Bob', 'Charlie', 'David', 'Eve', 'Frank', 'Grace', 'Heidi', 'Ivan', 'Judy',
            'Liam', 'Olivia', 'Noah', 'Emma', 'Jackson', 'Ava', 'Lucas', 'Sophia', 'Aiden', 'Mia',
            'Caleb', 'Zoe', 'Logan', 'Lily', 'Ethan', 'Chloe', 'Mason', 'Ella', 'James', 'Harper',
            'Benjamin', 'Scarlett', 'Daniel', 'Victoria', 'William', 'Grace', 'Henry', 'Nora',
            'Alexander', 'Aria', 'Samuel', 'Isla', 'Michael', 'Abigail', 'Elijah', 'Elizabeth',
            'Leo', 'Sofia', 'Owen', 'Luna', 'Julian', 'Aurora', 'Gabriel', 'Ellie', 'Andrew', 'Violet',
            'Jonathan', 'Stella', 'Nathan', 'Lucy', 'Joseph', 'Hazel', 'Christian', 'Natalie',
            'Ryan', 'Savannah', 'Nicholas', 'Penelope', 'David', 'Layla', 'Cameron', 'Brooklyn',
            'Adrian', 'Skylar', 'Sebastian', 'Audrey', 'Anthony', 'Claire', 'Eric', 'Anna',
            'Aaron', 'Maya', 'Adam', 'Eva', 'Joshua', 'Sarah', 'Kevin', 'Leah', 'Patrick', 'Ruby',
            'Jeremy', 'Hannah', 'Paul', 'Sophie', 'Stephen', 'Mila', 'Timothy', 'Eleanor',
            'Charles', 'Caroline', 'Peter', 'Madelyn', 'Robert', 'Alexa', 'Larry', 'Bella',
            'George', 'Julia', 'Walter', 'Alice', 'Oscar', 'Kira', 'Brenda', 'Diana', 'Felix',
            'Gina', 'Harry', 'Ivy', 'Jack', 'Karen', 'Louis', 'Megan', 'Oliver', 'Pamela',
            'Quinn', 'Rachel', 'Sam', 'Tina', 'Ulysses', 'Vicky', 'William', 'Xenia', 'Yara', 'Zack'
    ])[FLOOR(RANDOM() * 100) + 1] || ' ' ||
    (ARRAY['Smith', 'Jones', 'Williams', 'Brown', 'Davis', 'Miller', 'Wilson', 'Moore', 'Taylor', 'Anderson',
            'Thomas', 'Jackson', 'White', 'Harris', 'Martin', 'Thompson', 'Garcia', 'Martinez', 'Robinson', 'Clark',
            'Rodriguez', 'Lewis', 'Lee', 'Walker', 'Hall', 'Allen', 'Young', 'Hernandez', 'King', 'Wright',
            'Lopez', 'Hill', 'Scott', 'Green', 'Adams', 'Baker', 'Nelson', 'Carter', 'Mitchell', 'Perez',
            'Roberts', 'Turner', 'Phillips', 'Campbell', 'Parker', 'Evans', 'Edwards', 'Collins', 'Stewart', 'Sanchez',
            'Morris', 'Rogers', 'Reed', 'Cook', 'Morgan', 'Bell', 'Murphy', 'Bailey', 'Rivera', 'Cooper',
            'Richardson', 'Cox', 'Howard', 'Ward', 'Torres', 'Peterson', 'Gray', 'Ramirez', 'James', 'Watson',
            'Brooks', 'Kelly', 'Sanders', 'Price', 'Bennett', 'Wood', 'Barnes', 'Ross', 'Henderson', 'Coleman',
            'Jenkins', 'Perry', 'Powell', 'Long', 'Patterson', 'Hughes', 'Flores', 'Washington', 'Butler', 'Simmons',
            'Foster', 'Gonzales', 'Bryant', 'Alexander', 'Russell', 'Griffin', 'Diaz', 'Hayes', 'Myers', 'Ford'
    ])[FLOOR(RANDOM() * 100) + 1] || ' ' || LPAD(s.id::TEXT, 2, '0'), -- ensures uniqueness
    -- employee_contact
    '+91' || LPAD(FLOOR(RANDOM() * 9000000000 + 1000000000)::TEXT, 10, '0'),
    -- employee_location (Chennai areas)
    (ARRAY['Adyar', 'Anna Nagar', 'Velachery', 'Tambaram', 'Perungudi', 'OMR', 'Guindy', 'T. Nagar', 'Mylapore', 'Nungambakkam',
            'Pallavaram', 'Chromepet', 'Madipakkam', 'Ambattur', 'Porur', 'Thiruvanmiyur', 'Besant Nagar', 'Ashok Nagar', 'Kodambakkam', 'Royapettah'
    ])[FLOOR(RANDOM() * 20) + 1],
    -- age
    FLOOR(RANDOM() * (60 - 20 + 1) + 20)::INT,
    -- gender
    (ARRAY['Male', 'Female', 'Other'])[FLOOR(RANDOM() * 3) + 1],
    -- employee_position
    (CASE FLOOR(RANDOM() * 5)
        WHEN 0 THEN 'Software Engineer'
        WHEN 1 THEN 'Data Analyst'
        WHEN 2 THEN 'HR Specialist'
        WHEN 3 THEN 'Sales Executive'
        ELSE 'Project Manager'
    END),
    -- employee_branch
    (ARRAY['Chennai Central', 'Chennai South', 'Chennai North', 'Chennai IT Hub'])[FLOOR(RANDOM() * 4) + 1],
    -- reporting_employer_id, reporting_employer_name, reporting_employer_position
    (SELECT manager_data.mgr_id FROM (VALUES
        ('MGR001', 'Anil Kumar', 'Senior Project Manager'),
        ('MGR002', 'Bhavana Devi', 'Head of HR'),
        ('MGR003', 'Chaitanya Rao', 'Director of Sales'),
        ('MGR004', 'Deepa Sharma', 'Lead Data Scientist'),
        ('MGR005', 'Gautham Reddy', 'IT Director')
    ) AS manager_data(mgr_id, mgr_name, mgr_pos) ORDER BY RANDOM() LIMIT 1),
    (SELECT manager_data.mgr_name FROM (VALUES
        ('MGR001', 'Anil Kumar', 'Senior Project Manager'),
        ('MGR002', 'Bhavana Devi', 'Head of HR'),
        ('MGR003', 'Chaitanya Rao', 'Director of Sales'),
        ('MGR004', 'Deepa Sharma', 'Lead Data Scientist'),
        ('MGR005', 'Gautham Reddy', 'IT Director')
    ) AS manager_data(mgr_id, mgr_name, mgr_pos) ORDER BY RANDOM() LIMIT 1),
    (SELECT manager_data.mgr_pos FROM (VALUES
        ('MGR001', 'Anil Kumar', 'Senior Project Manager'),
        ('MGR002', 'Bhavana Devi', 'Head of HR'),
        ('MGR003', 'Chaitanya Rao', 'Director of Sales'),
        ('MGR004', 'Deepa Sharma', 'Lead Data Scientist'),
        ('MGR005', 'Gautham Reddy', 'IT Director')
    ) AS manager_data(mgr_id, mgr_name, mgr_pos) ORDER BY RANDOM() LIMIT 1),
    -- hire_date (between 2015 and 2024)
    (NOW() - (FLOOR(RANDOM() * 365 * 10) + 365)::INT * '1 day'::INTERVAL)::DATE,
    -- termination_date (15% attrition rate)
    CASE
        WHEN RANDOM() < 0.15 THEN (NOW() - (FLOOR(RANDOM() * 365) + 30)::INT * '1 day'::INTERVAL)::DATE -- within the last year, at least 30 days ago
        ELSE NULL
    END,
    -- is_active
    CASE
        WHEN RANDOM() < 0.15 THEN FALSE -- Corresponds to terminated employees
        ELSE TRUE
    END,
    -- days_present_last_year, days_absent_last_year, total_working_days_last_year
    -- Assume 260 total working days in a year
    ROUND(260 * (0.8 + RANDOM() * 0.2))::INT, -- 80-100% present
    ROUND(260 * (0.005 + RANDOM() * 0.1))::INT, -- 0.5-10% absent
    260,
    -- attendance_percentage (calculated)
    -- This will be calculated in a subquery or by the dashboard, but for insertion, we can derive a value
    (ROUND(260 * (0.8 + RANDOM() * 0.2))::INT * 100.0 / 260)::DECIMAL(5,2),
    -- reason_for_attrition
    CASE
        WHEN RANDOM() < 0.15 THEN (ARRAY['Better Opportunity', 'Relocation', 'Career Change', 'Personal Reasons', 'Performance Issues', 'Retirement', 'Company Layoff'])[FLOOR(RANDOM() * 7) + 1]
        ELSE NULL
    END,
    -- monthly_salary
    (CASE FLOOR(RANDOM() * 5)
        WHEN 0 THEN FLOOR(RANDOM() * (80000 - 40000 + 1) + 40000) -- Software Engineer/Data Analyst
        WHEN 1 THEN FLOOR(RANDOM() * (70000 - 35000 + 1) + 35000) -- HR Specialist/Sales Executive
        ELSE FLOOR(RANDOM() * (120000 - 90000 + 1) + 90000) -- Project Manager
    END)::DECIMAL(10,2),
    -- years_of_service (calculated from hire_date)
    EXTRACT(YEAR FROM AGE(NOW(), (NOW() - (FLOOR(RANDOM() * 365 * 10) + 365)::INT * '1 day'::INTERVAL)::DATE))::INT
FROM
    generate_series(1, 1000) AS s(id);

-- Optional: Update attendance_percentage and years_of_service based on actual values after insert
-- This is often done at the dashboard level or as a daily/weekly job, but included for completeness.
UPDATE employee_dashboard_data
SET
    attendance_percentage = ROUND((days_present_last_year * 100.0 / total_working_days_last_year), 2),
    years_of_service = EXTRACT(YEAR FROM AGE(
        CASE WHEN termination_date IS NOT NULL THEN termination_date ELSE NOW() END,
        hire_date
    ));
select * from employee_dashboard_data;
