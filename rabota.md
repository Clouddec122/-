Создание таблиц
REATE TABLE Faculties (
    faculty_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);
![123](https://github.com/Clouddec122/-/raw/main/123.png)


CREATE TABLE Departments (
    department_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    faculty_id INT REFERENCES Faculties(faculty_id)
);

CREATE TABLE Groups (
    group_id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    department_id INT REFERENCES Departments(department_id)
);

CREATE TABLE Students (
    student_id SERIAL PRIMARY KEY,
    full_name VARCHAR(100),
    birth_date DATE,
    group_id INT REFERENCES Groups(group_id)
);

CREATE TABLE Professors (
    professor_id SERIAL PRIMARY KEY,
    full_name VARCHAR(100),
    department_id INT REFERENCES Departments(department_id)
);

CREATE TABLE Courses (
    course_id SERIAL PRIMARY KEY,
    title VARCHAR(100),
    credits INT NOT NULL
);

CREATE TABLE Course_Professors (
    course_id INT REFERENCES Courses(course_id),
    professor_id INT REFERENCES Professors(professor_id),
    PRIMARY KEY(course_id, professor_id)
);

CREATE TABLE Enrollments (
    enrollment_id SERIAL PRIMARY KEY,
    student_id INT REFERENCES Students(student_id),
    course_id INT REFERENCES Courses(course_id),
    semester VARCHAR(10)
);

CREATE TABLE Exams (
    exam_id SERIAL PRIMARY KEY,
    course_id INT REFERENCES Courses(course_id),
    exam_date DATE,
    location VARCHAR(100)
);

CREATE TABLE Grades (
    grade_id SERIAL PRIMARY KEY,
    student_id INT REFERENCES Students(student_id),
    exam_id INT REFERENCES Exams(exam_id),
    grade NUMERIC(4,2)
);

CREATE TABLE Classrooms (
    classroom_id SERIAL PRIMARY KEY,
    name VARCHAR(50),
    capacity INT
);

CREATE TABLE Schedules (
    schedule_id SERIAL PRIMARY KEY,
    course_id INT REFERENCES Courses(course_id),
    classroom_id INT REFERENCES Classrooms(classroom_id),
    start_time TIME,
    end_time TIME,
    day_of_week VARCHAR(10)
);

CREATE TABLE Attendance (
    attendance_id SERIAL PRIMARY KEY,
    student_id INT REFERENCES Students(student_id),
    schedule_id INT REFERENCES Schedules(schedule_id),
    attended BOOLEAN
);

CREATE TABLE Assignments (
    assignment_id SERIAL PRIMARY KEY,
    course_id INT REFERENCES Courses(course_id),
    title VARCHAR(100),
    due_date DATE
);

CREATE TABLE Assignment_Submissions (
    submission_id SERIAL PRIMARY KEY,
    assignment_id INT REFERENCES Assignments(assignment_id),
    student_id INT REFERENCES Students(student_id),
    submission_date DATE,
    grade NUMERIC(4,2)
);

Заполнение данными 

INSERT INTO Faculties (name) VALUES 
('Faculty of Science'),
('Faculty of Arts'),
('Faculty of Engineering');

INSERT INTO Departments (name, faculty_id) VALUES 
('Mathematics', 1),
('Physics', 1),
('History', 2),
('Computer Science', 3),
('Electrical Engineering', 3);

INSERT INTO Groups (name, department_id) VALUES 
('Group A', 1),
('Group B', 2),
('Group C', 4),
('Group D', 5);

INSERT INTO Students (full_name, birth_date, group_id) VALUES 
('Alice Johnson', '2002-05-14', 1),
('Bob Smith', '2001-03-22', 2),
('Charlie Brown', '2003-07-09', 3),
('Diana Prince', '2002-11-30', 4);

INSERT INTO Professors (full_name, department_id) VALUES 
('Dr. Alan Turing', 4),
('Dr. Marie Curie', 2),
('Dr. Isaac Newton', 1),
('Dr. Ada Lovelace', 5);

INSERT INTO Courses (title, credits) VALUES 
('Calculus I', 4),
('Modern Physics', 3),
('Data Structures', 4),
('Electrical Circuits', 3);

INSERT INTO Course_Professors (course_id, professor_id) VALUES 
(1, 3),
(2, 2),
(3, 1),
(4, 4);

INSERT INTO Enrollments (student_id, course_id, semester) VALUES 
(1, 1, 'Fall'),
(2, 2, 'Fall'),
(3, 3, 'Fall'),
(4, 4, 'Fall');

INSERT INTO Exams (course_id, exam_date, location) VALUES 
(1, '2025-12-01', 'Room 101'),
(2, '2025-12-02', 'Room 102'),
(3, '2025-12-03', 'Room 103'),
(4, '2025-12-04', 'Room 104');

INSERT INTO Grades (student_id, exam_id, grade) VALUES 
(1, 1, 88.50),
(2, 2, 79.00),
(3, 3, 91.75),
(4, 4, 85.00);

INSERT INTO Classrooms (name, capacity) VALUES 
('Room 101', 30),
('Room 102', 40),
('Room 103', 25),
('Room 104', 35);

INSERT INTO Schedules (course_id, classroom_id, start_time, end_time, day_of_week) VALUES 
(1, 1, '09:00', '10:30', 'Monday'),
(2, 2, '11:00', '12:30', 'Tuesday'),
(3, 3, '13:00', '14:30', 'Wednesday'),
(4, 4, '15:00', '16:30', 'Thursday');

INSERT INTO Attendance (student_id, schedule_id, attended) VALUES 
(1, 1, TRUE),
(2, 2, FALSE),
(3, 3, TRUE),
(4, 4, TRUE);

INSERT INTO Assignments (course_id, title, due_date) VALUES 
(1, 'Limits and Derivatives', '2025-10-01'),
(2, 'Quantum Theory Essay', '2025-10-05'),
(3, 'Binary Trees Implementation', '2025-10-10'),
(4, 'Resistor Calculations', '2025-10-15');

INSERT INTO Assignment_Submissions (assignment_id, student_id, submission_date, grade) VALUES 
(1, 1, '2025-09-30', 92.00),
(2, 2, '2025-10-04', 78.50),
(3, 3, '2025-10-09', 88.00),
(4, 4, '2025-10-14', 85.50);


Представления 

CREATE VIEW student_courses_view AS
SELECT s.student_id, s.full_name, c.title AS course
FROM Students s
JOIN Enrollments e ON s.student_id = e.student_id
JOIN Courses c ON e.course_id = c.course_id;

CREATE MATERIALIZED VIEW course_enrollment_counts AS
SELECT c.course_id, c.title, COUNT(e.student_id) AS student_count
FROM Courses c
LEFT JOIN Enrollments e ON c.course_id = e.course_id
GROUP BY c.course_id;


Хранимые процедуры 


CREATE OR REPLACE FUNCTION enroll_student(p_student INT, p_course INT, p_semester TEXT)
RETURNS VOID AS $$
BEGIN
    INSERT INTO Enrollments(student_id, course_id, semester)
    VALUES (p_student, p_course, p_semester);
END;
$$ LANGUAGE plpgsql;

CREATE OR REPLACE FUNCTION add_grade(p_student INT, p_exam INT, p_grade NUMERIC)
RETURNS VOID AS $$
BEGIN
    INSERT INTO Grades(student_id, exam_id, grade)
    VALUES (p_student, p_exam, p_grade);
END;
$$ LANGUAGE plpgsql;

CREATE OR REPLACE FUNCTION mark_attendance(p_student INT, p_schedule INT, p_attended BOOLEAN)
RETURNS VOID AS $$
BEGIN
    INSERT INTO Attendance(student_id, schedule_id, attended)
    VALUES (p_student, p_schedule, p_attended);
END;
$$ LANGUAGE plpgsql;

CREATE OR REPLACE FUNCTION submit_assignment(p_student INT, p_assignment INT, p_date DATE)
RETURNS VOID AS $$
BEGIN
    INSERT INTO Assignment_Submissions(student_id, assignment_id, submission_date)
    VALUES (p_student, p_assignment, p_date);
END;
$$ LANGUAGE plpgsql;

CREATE OR REPLACE FUNCTION create_course(p_title TEXT, p_credits INT)
RETURNS VOID AS $$
BEGIN
    INSERT INTO Courses(title, credits)
    VALUES (p_title, p_credits);
END;
$$ LANGUAGE plpgsql;


Триггеры на CRUD

CREATE OR REPLACE FUNCTION log_enrollment()
RETURNS TRIGGER AS $$
BEGIN
    IF TG_OP = 'INSERT' THEN
        RAISE NOTICE 'Student % enrolled in course %', NEW.student_id, NEW.course_id;
    ELSIF TG_OP = 'UPDATE' THEN
        RAISE NOTICE 'Enrollment updated: %', NEW.enrollment_id;
    ELSIF TG_OP = 'DELETE' THEN
        RAISE NOTICE 'Enrollment removed: %', OLD.enrollment_id;
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trigger_log_enrollment
AFTER INSERT OR UPDATE OR DELETE ON Enrollments
FOR EACH ROW EXECUTE FUNCTION log_enrollment
