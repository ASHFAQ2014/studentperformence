-- Step 1: Create tables

CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100),
    gender CHAR(1),
    date_of_birth DATE,
    class VARCHAR(10)
);

CREATE TABLE Subjects (
    subject_id INT PRIMARY KEY,
    subject_name VARCHAR(100)
);

CREATE TABLE Marks (
    mark_id INT PRIMARY KEY,
    student_id INT,
    subject_id INT,
    marks_obtained INT,
    exam_type VARCHAR(50),
    exam_date DATE,
    FOREIGN KEY (student_id) REFERENCES Students(student_id),
    FOREIGN KEY (subject_id) REFERENCES Subjects(subject_id)
);

-- Step 2: Insert sample data into Students table

INSERT INTO Students VALUES
(1, 'Aisha Khan', 'F', '2006-05-12', '10A'),
(2, 'Ravi Kumar', 'M', '2005-11-04', '10A'),
(3, 'Sneha Das', 'F', '2006-01-23', '10B'),
(4, 'Arjun Mehta', 'M', '2005-07-17', '10B'),
(5, 'Fatima Noor', 'F', '2006-08-10', '10A');

-- Step 3: Insert sample data into Subjects table

INSERT INTO Subjects VALUES
(1, 'Math'),
(2, 'Science'),
(3, 'English'),
(4, 'History'),
(5, 'Geography');

-- Step 4: Insert marks of students

INSERT INTO Marks VALUES
(1, 1, 1, 88, 'Midterm', '2025-01-10'),
(2, 1, 2, 75, 'Midterm', '2025-01-11'),
(3, 2, 1, 68, 'Midterm', '2025-01-10'),
(4, 2, 2, 71, 'Midterm', '2025-01-11'),
(5, 3, 1, 91, 'Midterm', '2025-01-10'),
(6, 3, 3, 76, 'Midterm', '2025-01-12'),
(7, 4, 2, 59, 'Midterm', '2025-01-11'),
(8, 5, 1, 83, 'Midterm', '2025-01-10');

-- Step 5: View all students with their marks

SELECT Students.name, Subjects.subject_name, Marks.marks_obtained
FROM Students
JOIN Marks ON Students.student_id = Marks.student_id
JOIN Subjects ON Marks.subject_id = Subjects.subject_id;

-- Step 6: Find average marks for each subject

SELECT Subjects.subject_name, AVG(Marks.marks_obtained) AS average_marks
FROM Marks
JOIN Subjects ON Marks.subject_id = Subjects.subject_id
GROUP BY Subjects.subject_name;

-- Step 7: Find class-wise average marks

SELECT Students.class, AVG(Marks.marks_obtained) AS class_average
FROM Students
JOIN Marks ON Students.student_id = Marks.student_id
GROUP BY Students.class;

-- Step 8: Create a view to show total and average marks for each student

-- A view is like a virtual table that shows result of a query
-- This view will show student's name, class, total marks, and average marks

CREATE VIEW StudentPerformance AS
SELECT 
    Students.name, 
    Students.class,
    SUM(Marks.marks_obtained) AS total_marks,       -- adding all marks of the student
    AVG(Marks.marks_obtained) AS average_marks      -- calculating average of all marks
FROM Students
JOIN Marks ON Students.student_id = Marks.student_id
GROUP BY Students.name, Students.class;

-- Step 9: Display the view

select* from studentperformance
