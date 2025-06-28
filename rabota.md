Создание таблиц
```sql
REATE TABLE Faculties (
    faculty_id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL
);
```
![123](https://github.com/Clouddec122/-/raw/main/123.png)

![2](https://github.com/Clouddec122/-/raw/main/2.png)
```sql
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
```
Заполнение данными 
```sql
INSERT INTO Faculties (name) VALUES 
('Faculty of Science'),
('Faculty of Arts'),
('Faculty of Engineering');
```
![2](https://github.com/Clouddec122/-/raw/main/2.png)
```sql
INSERT INTO Departments (name, faculty_id) VALUES 
('Mathematics', 1),
('Physics', 1),
('History', 2),
('Computer Science', 3),
('Electrical Engineering', 3);
```
![3](https://github.com/Clouddec122/-/raw/main/3.png)

```sql
INSERT INTO Groups (name, department_id) VALUES 
('Group A', 1),
('Group B', 2),
('Group C', 4),
('Group D', 5);
```
![4](https://github.com/Clouddec122/-/raw/main/4.png)
```sql
INSERT INTO Students (full_name, birth_date, group_id) VALUES 
('Ilja Motorov', '2002-05-14', 1),
('Semen Kuznecov', '2001-03-22', 2),
('Anton Kalmikov', '2003-07-09', 3),
('Diana Krimova', '2002-11-30', 4);
```
![5](https://github.com/Clouddec122/-/raw/main/5.png)

```sql
INSERT INTO Professors (full_name, department_id) VALUES 
('Dr. Anton Turing', 4),
('Dr. Karl Curie', 2),
('Dr. Tomass Newton', 1),
('Dr. Lida Lovelace', 5);
```

![6](https://github.com/Clouddec122/-/raw/main/6.png)

```sql
INSERT INTO Courses (title, credits) VALUES 
('Calculus I', 4),
('Modern Physics', 3),
('Data Structures', 4),
('Electrical Circuits', 3);
```

![7](https://github.com/Clouddec122/-/raw/main/7.png)

```sql
INSERT INTO Course_Professors (course_id, professor_id) VALUES 
(1, 3),
(2, 2),
(3, 1),
(4, 4);
```

![8](https://github.com/Clouddec122/-/raw/main/8.png)

```sql
INSERT INTO Enrollments (student_id, course_id, semester) VALUES 
(1, 1, 'Fall'),
(2, 2, 'Fall'),
(3, 3, 'Fall'),
(4, 4, 'Fall');
```

![9](https://github.com/Clouddec122/-/raw/main/9.png)

```sql
INSERT INTO Exams (course_id, exam_date, location) VALUES 
(1, '2025-12-01', 'Room 101'),
(2, '2025-12-02', 'Room 102'),
(3, '2025-12-03', 'Room 103'),
(4, '2025-12-04', 'Room 104');
```

![10](https://github.com/Clouddec122/-/raw/main/10.png)

```sql
INSERT INTO Grades (student_id, exam_id, grade) VALUES 
(1, 1, 88.50),
(2, 2, 79.00),
(3, 3, 91.75),
(4, 4, 85.00);
```

![11](https://github.com/Clouddec122/-/raw/main/11.png)

```sql
INSERT INTO Classrooms (name, capacity) VALUES 
('Room 101', 30),
('Room 102', 40),
('Room 103', 25),
('Room 104', 35);
```

![12](https://github.com/Clouddec122/-/raw/main/12.png)

```sql
INSERT INTO Schedules (course_id, classroom_id, start_time, end_time, day_of_week) VALUES 
(1, 1, '09:00', '10:30', 'Monday'),
(2, 2, '11:00', '12:30', 'Tuesday'),
(3, 3, '13:00', '14:30', 'Wednesday'),
(4, 4, '15:00', '16:30', 'Thursday');
```

![13](https://github.com/Clouddec122/-/raw/main/13.png)

```sql
INSERT INTO Attendance (student_id, schedule_id, attended) VALUES 
(1, 1, TRUE),
(2, 2, FALSE),
(3, 3, TRUE),
(4, 4, TRUE);
```

![14](https://github.com/Clouddec122/-/raw/main/14.png)

```sql
INSERT INTO Assignments (course_id, title, due_date) VALUES 
(1, 'Limits and Derivatives', '2025-10-01'),
(2, 'Quantum Theory Essay', '2025-10-05'),
(3, 'Binary Trees Implementation', '2025-10-10'),
(4, 'Resistor Calculations', '2025-10-15');
```

![15](https://github.com/Clouddec122/-/raw/main/15.png)

```sql
INSERT INTO Assignment_Submissions (assignment_id, student_id, submission_date, grade) VALUES 
(1, 1, '2025-09-30', 92.00),
(2, 2, '2025-10-04', 78.50),
(3, 3, '2025-10-09', 88.00),
(4, 4, '2025-10-14', 85.50);
```

![16](https://github.com/Clouddec122/-/raw/main/16.png)


Представления 
```sql
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
```
![17](https://github.com/Clouddec122/-/raw/main/17.png)


Хранимые процедуры 

```sql
CREATE OR REPLACE FUNCTION enroll_student(p_student INT, p_course INT, p_semester TEXT)
RETURNS VOID AS $$
BEGIN
    INSERT INTO Enrollments(student_id, course_id, semester)
    VALUES (p_student, p_course, p_semester);
END;
$$ LANGUAGE plpgsql;
```
![18](https://github.com/Clouddec122/-/raw/main/18.png)

```sql
CREATE OR REPLACE FUNCTION add_grade(p_student INT, p_exam INT, p_grade NUMERIC)
RETURNS VOID AS $$
BEGIN
    INSERT INTO Grades(student_id, exam_id, grade)
    VALUES (p_student, p_exam, p_grade);
END;
$$ LANGUAGE plpgsql;
```

![19](https://github.com/Clouddec122/-/raw/main/19.png)

```sql
CREATE OR REPLACE FUNCTION mark_attendance(p_student INT, p_schedule INT, p_attended BOOLEAN)
RETURNS VOID AS $$
BEGIN
    INSERT INTO Attendance(student_id, schedule_id, attended)
    VALUES (p_student, p_schedule, p_attended);
END;
$$ LANGUAGE plpgsql;
```

![20](https://github.com/Clouddec122/-/raw/main/20.png)

```sql
CREATE OR REPLACE FUNCTION submit_assignment(p_student INT, p_assignment INT, p_date DATE)
RETURNS VOID AS $$
BEGIN
    INSERT INTO Assignment_Submissions(student_id, assignment_id, submission_date)
    VALUES (p_student, p_assignment, p_date);
END;
$$ LANGUAGE plpgsql;
```

![21](https://github.com/Clouddec122/-/raw/main/21.png)

```sql
CREATE OR REPLACE FUNCTION create_course(p_title TEXT, p_credits INT)
RETURNS VOID AS $$
BEGIN
    INSERT INTO Courses(title, credits)
    VALUES (p_title, p_credits);
END;
$$ LANGUAGE plpgsql;
```

![22](https://github.com/Clouddec122/-/raw/main/22.png)


Триггеры на CRUD
```sql
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
```
![24](https://github.com/Clouddec122/-/raw/main/24.png)
![25](https://github.com/Clouddec122/-/raw/main/25.png)



-- Все факультеты
```sql
SELECT * FROM Faculties;
```
![26](https://github.com/Clouddec122/-/raw/main/26.png)

-- Все департаменты с названием факультета
```sql
SELECT d.department_id, d.name, f.name AS faculty_name
FROM Departments d
JOIN Faculties f ON d.faculty_id = f.faculty_id;
```
![27](https://github.com/Clouddec122/-/raw/main/27.png)


-- Все студенты с группами
```sql
SELECT s.student_id, s.full_name, g.name AS group_name
FROM Students s
JOIN Groups g ON s.group_id = g.group_id;
```
![28](https://github.com/Clouddec122/-/raw/main/28.png)


-- Все курсы с преподавателями
```sql
SELECT c.title, p.full_name AS professor
FROM Courses c
JOIN Course_Professors cp ON c.course_id = cp.course_id
JOIN Professors p ON cp.professor_id = p.professor_id;
```

![29](https://github.com/Clouddec122/-/raw/main/29.png)

```sql
SELECT * FROM student_courses_view;
SELECT * FROM course_enrollment_counts;
REFRESH MATERIALIZED VIEW course_enrollment_counts;
-- Запишем нового студента на курс
SELECT enroll_student(1, 2, 'Spring');
```

![30](https://github.com/Clouddec122/-/raw/main/30.png)


-- Добавим новую оценку студенту
```sql
SELECT add_grade(1, 2, 95.5);
```


-- Отметим посещаемость
```sql
SELECT mark_attendance(1, 2, TRUE);
```
![31](https://github.com/Clouddec122/-/raw/main/31.png)


-- Сдача задания
```sql
SELECT submit_assignment(1, 2, CURRENT_DATE);
```
-- Создадим новый курс
```sql
SELECT create_course('Philosophy 101', 3);
```
![32](https://github.com/Clouddec122/-/raw/main/32.png)
```sql
SELECT * FROM Enrollments WHERE student_id = 1;
SELECT * FROM Grades WHERE student_id = 1;
SELECT * FROM Attendance WHERE student_id = 1;
SELECT * FROM Assignment_Submissions WHERE student_id = 1;
SELECT * FROM Courses WHERE title = 'Philosophy 101';
```
![33](https://github.com/Clouddec122/-/raw/main/33.png)

