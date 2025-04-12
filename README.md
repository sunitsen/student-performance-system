<pre>
-- Create the subjects table
CREATE TABLE subjects (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

-- Create the students table
CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);

-- Create the student_subjects table (many-to-many relationship)
CREATE TABLE student_subjects (
    student_id INT,
    subject_id INT,
    PRIMARY KEY (student_id, subject_id),
    FOREIGN KEY (student_id) REFERENCES students(id) ON DELETE CASCADE,
    FOREIGN KEY (subject_id) REFERENCES subjects(id) ON DELETE CASCADE
);

-- Create the marks table
CREATE TABLE marks (
    student_id INT,
    subject_id INT,
    marks INT CHECK (marks BETWEEN 0 AND 100),
    PRIMARY KEY (student_id, subject_id),
    FOREIGN KEY (student_id) REFERENCES students(id) ON DELETE CASCADE,
    FOREIGN KEY (subject_id) REFERENCES subjects(id) ON DELETE CASCADE
);

-- Insert custom subjects
INSERT INTO subjects (name) VALUES
('Data Structures'),
('Algorithms'),
('Web Development'),
('Databases'),
('Software Engineering');

-- Insert custom students
INSERT INTO students (name) VALUES
('Sunit Das'), ('Ravi Mehra'), ('Ayesha Khan'), ('Tariq Aziz'), ('Nina Patel'),
('Rahul Verma'), ('Zoya Sheikh'), ('Kabir Ali'), ('Ananya Roy'), ('Mohan Lal'),
('Deepa Joshi'), ('Imran Hussain'), ('Leela Nair'), ('Kunal Chopra'), ('Meena Iyer'),
('Aditya Shah'), ('Pooja Sethi'), ('Harsh Gupta'), ('Ritika Sen'), ('Yash Dubey'),
('Sana Rafiq'), ('Rohan Malhotra'), ('Neha Kapoor'), ('Aman Khanna'), ('Divya Jain'),
('Ritu Raj'), ('Alok Sinha'), ('Simran Oberoi'), ('Manav Singh'), ('Tanya Batra'),
('Pranav Mittal'), ('Isha Arora'), ('Aryan Bhatia'), ('Sneha Das'), ('Jay Mehta'),
('Bhavna Rawat'), ('Zaid Mirza'), ('Avni Rao'), ('Nikhil Saxena'), ('Tanvi Chawla');

-- Assign up to three random subjects per student
INSERT INTO student_subjects (student_id, subject_id)
SELECT student_id, subject_id FROM (
    SELECT s.id AS student_id, sub.id AS subject_id, ROW_NUMBER() OVER (PARTITION BY s.id ORDER BY RAND()) AS rn
    FROM students s CROSS JOIN subjects sub
) t WHERE t.rn <= 3;

-- Insert random marks between 40 and 100
INSERT INTO marks (student_id, subject_id, marks)
SELECT ss.student_id, ss.subject_id, FLOOR(RAND() * 61) + 40
FROM student_subjects ss;

</pre>
