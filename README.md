CREATE TABLE Users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role ENUM('student', 'instructor', 'admin') DEFAULT 'student',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE Courses (
    course_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(150) NOT NULL,
    description TEXT,
    instructor_id INT NOT NULL,
    price DECIMAL(10,2) DEFAULT 0.00,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (instructor_id) REFERENCES Users(user_id) ON DELETE CASCADE
);

CREATE TABLE Enrollments (
    enrollment_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    course_id INT NOT NULL,
    enrolled_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status ENUM('active', 'completed', 'dropped') DEFAULT 'active',
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

CREATE TABLE Modules (
    module_id INT AUTO_INCREMENT PRIMARY KEY,
    course_id INT NOT NULL,
    title VARCHAR(150) NOT NULL,
    content TEXT,
    position INT NOT NULL,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

CREATE TABLE Assignments (
    assignment_id INT AUTO_INCREMENT PRIMARY KEY,
    course_id INT NOT NULL,
    title VARCHAR(150) NOT NULL,
    description TEXT,
    due_date DATE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

CREATE TABLE Submissions (
    submission_id INT AUTO_INCREMENT PRIMARY KEY,
    assignment_id INT NOT NULL,
    student_id INT NOT NULL,
    submitted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    grade DECIMAL(5,2),
    feedback TEXT,
    FOREIGN KEY (assignment_id) REFERENCES Assignments(assignment_id) ON DELETE CASCADE,
    FOREIGN KEY (student_id) REFERENCES Users(user_id) ON DELETE CASCADE
);

CREATE TABLE Quizzes (
    quiz_id INT AUTO_INCREMENT PRIMARY KEY,
    course_id INT NOT NULL,
    title VARCHAR(150) NOT NULL,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

CREATE TABLE Questions (
    question_id INT AUTO_INCREMENT PRIMARY KEY,
    quiz_id INT NOT NULL,
    question_text TEXT NOT NULL,
    option_a VARCHAR(255),
    option_b VARCHAR(255),
    option_c VARCHAR(255),
    option_d VARCHAR(255),
    correct_option ENUM('A','B','C','D') NOT NULL,
    FOREIGN KEY (quiz_id) REFERENCES Quizzes(quiz_id) ON DELETE CASCADE
);

CREATE TABLE QuizAttempts (
    attempt_id INT AUTO_INCREMENT PRIMARY KEY,
    quiz_id INT NOT NULL,
    student_id INT NOT NULL,
    score DECIMAL(5,2),
    attempted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (quiz_id) REFERENCES Quizzes(quiz_id) ON DELETE CASCADE,
    FOREIGN KEY (student_id) REFERENCES Users(user_id) ON DELETE CASCADE
);

CREATE TABLE Payments (
    payment_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    course_id INT NOT NULL,
    amount DECIMAL(10,2) NOT NULL,
    payment_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status ENUM('pending','completed','failed') DEFAULT 'pending',
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

INSERT INTO Users (full_name, email, password_hash, role) VALUES
('Alice Johnson', 'alice@example.com', 'hash123', 'student'),
('Bob Smith', 'bob@example.com', 'hash456', 'student'),
('Dr. Emily Brown', 'emily@example.com', 'hash789', 'instructor'),
('Admin User', 'admin@example.com', 'hash000', 'admin');

INSERT INTO Courses (title, description, instructor_id, price) VALUES
('Introduction to Python', 'Learn Python basics including syntax, data types, and control structures.', 3, 49.99),
('Web Development with HTML & CSS', 'Build responsive websites using HTML and CSS.', 3, 39.99);

INSERT INTO Enrollments (user_id, course_id, status) VALUES
(1, 1, 'active'),
(2, 1, 'active'),
(1, 2, 'active');

INSERT INTO Modules (course_id, title, content, position) VALUES
(1, 'Python Basics', 'Introduction to Python, variables, and data types.', 1),
(1, 'Control Structures', 'If-else statements, loops, and logical operators.', 2),
(2, 'HTML Fundamentals', 'Learn HTML tags, structure, and attributes.', 1),
(2, 'CSS Styling', 'Introduction to CSS selectors and properties.', 2);

INSERT INTO Assignments (course_id, title, description, due_date) VALUES
(1, 'Python Assignment 1', 'Write a program to calculate factorial of a number.', '2025-09-20'),
(2, 'HTML & CSS Project', 'Create a simple portfolio website.', '2025-09-25');

INSERT INTO Submissions (assignment_id, student_id, grade, feedback) VALUES
(1, 1, 95.5, 'Great work!'),
(1, 2, 88.0, 'Good job, but missing edge cases.');

INSERT INTO Quizzes (course_id, title) VALUES
(1, 'Python Basics Quiz'),
(2, 'HTML Fundamentals Quiz');

INSERT INTO Questions (quiz_id, question_text, option_a, option_b, option_c, option_d, correct_option) VALUES
(1, 'Which of the following is a valid variable name in Python?', '2var', '_name', 'class', 'for', 'B'),
(1, 'What is the output of print(2**3)?', '6', '8', '9', '16', 'B'),
(2, 'Which HTML tag is used for inserting a line break?', '<br>', '<hr>', '<p>', '<lb>', 'A');

INSERT INTO QuizAttempts (quiz_id, student_id, score) VALUES
(1, 1, 90.0),
(1, 2, 85.0);

INSERT INTO Payments (user_id, course_id, amount, status) VALUES
(1, 1, 49.99, 'completed'),
(1, 2, 39.99, 'completed'),
(2, 1, 49.99, 'completed');
SHOW TABLES;

DESCRIBE Users;
DESCRIBE Courses;
DESCRIBE Enrollments;
DESCRIBE Modules;
DESCRIBE Assignments;
DESCRIBE Submissions;
DESCRIBE Quizzes;
DESCRIBE Questions;
DESCRIBE QuizAttempts;
DESCRIBE Payments;

SELECT * FROM Users;
SELECT * FROM Courses;
SELECT * FROM Enrollments;
SELECT * FROM Modules;
SELECT * FROM Assignments;
SELECT * FROM Submissions;
SELECT * FROM Quizzes;
SELECT * FROM Questions;
SELECT * FROM QuizAttempts;
SELECT * FROM Payments;
CREATE TABLE Users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role ENUM('student', 'instructor', 'admin') DEFAULT 'student',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE Courses (
    course_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(150) NOT NULL,
    description TEXT,
    instructor_id INT NOT NULL,
    price DECIMAL(10,2) DEFAULT 0.00,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (instructor_id) REFERENCES Users(user_id) ON DELETE CASCADE
);

CREATE TABLE Enrollments (
    enrollment_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    course_id INT NOT NULL,
    enrolled_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status ENUM('active', 'completed', 'dropped') DEFAULT 'active',
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

CREATE TABLE Modules (
    module_id INT AUTO_INCREMENT PRIMARY KEY,
    course_id INT NOT NULL,
    title VARCHAR(150) NOT NULL,
    content TEXT,
    position INT NOT NULL,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

CREATE TABLE Assignments (
    assignment_id INT AUTO_INCREMENT PRIMARY KEY,
    course_id INT NOT NULL,
    title VARCHAR(150) NOT NULL,
    description TEXT,
    due_date DATE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

CREATE TABLE Submissions (
    submission_id INT AUTO_INCREMENT PRIMARY KEY,
    assignment_id INT NOT NULL,
    student_id INT NOT NULL,
    submitted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    grade DECIMAL(5,2),
    feedback TEXT,
    FOREIGN KEY (assignment_id) REFERENCES Assignments(assignment_id) ON DELETE CASCADE,
    FOREIGN KEY (student_id) REFERENCES Users(user_id) ON DELETE CASCADE
);

CREATE TABLE Quizzes (
    quiz_id INT AUTO_INCREMENT PRIMARY KEY,
    course_id INT NOT NULL,
    title VARCHAR(150) NOT NULL,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

CREATE TABLE Questions (
    question_id INT AUTO_INCREMENT PRIMARY KEY,
    quiz_id INT NOT NULL,
    question_text TEXT NOT NULL,
    option_a VARCHAR(255),
    option_b VARCHAR(255),
    option_c VARCHAR(255),
    option_d VARCHAR(255),
    correct_option ENUM('A','B','C','D') NOT NULL,
    FOREIGN KEY (quiz_id) REFERENCES Quizzes(quiz_id) ON DELETE CASCADE
);

CREATE TABLE QuizAttempts (
    attempt_id INT AUTO_INCREMENT PRIMARY KEY,
    quiz_id INT NOT NULL,
    student_id INT NOT NULL,
    score DECIMAL(5,2),
    attempted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (quiz_id) REFERENCES Quizzes(quiz_id) ON DELETE CASCADE,
    FOREIGN KEY (student_id) REFERENCES Users(user_id) ON DELETE CASCADE
);

CREATE TABLE Payments (
    payment_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    course_id INT NOT NULL,
    amount DECIMAL(10,2) NOT NULL,
    payment_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status ENUM('pending','completed','failed') DEFAULT 'pending',
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

INSERT INTO Users (full_name, email, password_hash, role) VALUES
('Alice Johnson', 'alice@example.com', 'hash123', 'student'),
('Bob Smith', 'bob@example.com', 'hash456', 'student'),
('Dr. Emily Brown', 'emily@example.com', 'hash789', 'instructor'),
('Admin User', 'admin@example.com', 'hash000', 'admin');

INSERT INTO Courses (title, description, instructor_id, price) VALUES
('Introduction to Python', 'Learn Python basics including syntax, data types, and control structures.', 3, 49.99),
('Web Development with HTML & CSS', 'Build responsive websites using HTML and CSS.', 3, 39.99);

INSERT INTO Enrollments (user_id, course_id, status) VALUES
(1, 1, 'active'),
(2, 1, 'active'),
(1, 2, 'active');

INSERT INTO Modules (course_id, title, content, position) VALUES
(1, 'Python Basics', 'Introduction to Python, variables, and data types.', 1),
(1, 'Control Structures', 'If-else statements, loops, and logical operators.', 2),
(2, 'HTML Fundamentals', 'Learn HTML tags, structure, and attributes.', 1),
(2, 'CSS Styling', 'Introduction to CSS selectors and properties.', 2);

INSERT INTO Assignments (course_id, title, description, due_date) VALUES
(1, 'Python Assignment 1', 'Write a program to calculate factorial of a number.', '2025-09-20'),
(2, 'HTML & CSS Project', 'Create a simple portfolio website.', '2025-09-25');

INSERT INTO Submissions (assignment_id, student_id, grade, feedback) VALUES
(1, 1, 95.5, 'Great work!'),
(1, 2, 88.0, 'Good job, but missing edge cases.');

INSERT INTO Quizzes (course_id, title) VALUES
(1, 'Python Basics Quiz'),
(2, 'HTML Fundamentals Quiz');

INSERT INTO Questions (quiz_id, question_text, option_a, option_b, option_c, option_d, correct_option) VALUES
(1, 'Which of the following is a valid variable name in Python?', '2var', '_name', 'class', 'for', 'B'),
(1, 'What is the output of print(2**3)?', '6', '8', '9', '16', 'B'),
(2, 'Which HTML tag is used for inserting a line break?', '<br>', '<hr>', '<p>', '<lb>', 'A');

INSERT INTO QuizAttempts (quiz_id, student_id, score) VALUES
(1, 1, 90.0),
(1, 2, 85.0);

INSERT INTO Payments (user_id, course_id, amount, status) VALUES
(1, 1, 49.99, 'completed'),
(1, 2, 39.99, 'completed'),
(2, 1, 49.99, 'completed');
SHOW TABLES;

DESCRIBE Users;
DESCRIBE Courses;
DESCRIBE Enrollments;
DESCRIBE Modules;
DESCRIBE Assignments;
DESCRIBE Submissions;
DESCRIBE Quizzes;
DESCRIBE Questions;
DESCRIBE QuizAttempts;
DESCRIBE Payments;

SELECT * FROM Users;
SELECT * FROM Courses;
SELECT * FROM Enrollments;
SELECT * FROM Modules;
SELECT * FROM Assignments;
SELECT * FROM Submissions;
SELECT * FROM Quizzes;
SELECT * FROM Questions;
SELECT * FROM QuizAttempts;
SELECT * FROM Payments;
CREATE TABLE Users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    full_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role ENUM('student', 'instructor', 'admin') DEFAULT 'student',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE Courses (
    course_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(150) NOT NULL,
    description TEXT,
    instructor_id INT NOT NULL,
    price DECIMAL(10,2) DEFAULT 0.00,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (instructor_id) REFERENCES Users(user_id) ON DELETE CASCADE
);

CREATE TABLE Enrollments (
    enrollment_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    course_id INT NOT NULL,
    enrolled_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status ENUM('active', 'completed', 'dropped') DEFAULT 'active',
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

CREATE TABLE Modules (
    module_id INT AUTO_INCREMENT PRIMARY KEY,
    course_id INT NOT NULL,
    title VARCHAR(150) NOT NULL,
    content TEXT,
    position INT NOT NULL,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

CREATE TABLE Assignments (
    assignment_id INT AUTO_INCREMENT PRIMARY KEY,
    course_id INT NOT NULL,
    title VARCHAR(150) NOT NULL,
    description TEXT,
    due_date DATE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

CREATE TABLE Submissions (
    submission_id INT AUTO_INCREMENT PRIMARY KEY,
    assignment_id INT NOT NULL,
    student_id INT NOT NULL,
    submitted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    grade DECIMAL(5,2),
    feedback TEXT,
    FOREIGN KEY (assignment_id) REFERENCES Assignments(assignment_id) ON DELETE CASCADE,
    FOREIGN KEY (student_id) REFERENCES Users(user_id) ON DELETE CASCADE
);

CREATE TABLE Quizzes (
    quiz_id INT AUTO_INCREMENT PRIMARY KEY,
    course_id INT NOT NULL,
    title VARCHAR(150) NOT NULL,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

CREATE TABLE Questions (
    question_id INT AUTO_INCREMENT PRIMARY KEY,
    quiz_id INT NOT NULL,
    question_text TEXT NOT NULL,
    option_a VARCHAR(255),
    option_b VARCHAR(255),
    option_c VARCHAR(255),
    option_d VARCHAR(255),
    correct_option ENUM('A','B','C','D') NOT NULL,
    FOREIGN KEY (quiz_id) REFERENCES Quizzes(quiz_id) ON DELETE CASCADE
);

CREATE TABLE QuizAttempts (
    attempt_id INT AUTO_INCREMENT PRIMARY KEY,
    quiz_id INT NOT NULL,
    student_id INT NOT NULL,
    score DECIMAL(5,2),
    attempted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (quiz_id) REFERENCES Quizzes(quiz_id) ON DELETE CASCADE,
    FOREIGN KEY (student_id) REFERENCES Users(user_id) ON DELETE CASCADE
);

CREATE TABLE Payments (
    payment_id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    course_id INT NOT NULL,
    amount DECIMAL(10,2) NOT NULL,
    payment_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status ENUM('pending','completed','failed') DEFAULT 'pending',
    FOREIGN KEY (user_id) REFERENCES Users(user_id) ON DELETE CASCADE,
    FOREIGN KEY (course_id) REFERENCES Courses(course_id) ON DELETE CASCADE
);

INSERT INTO Users (full_name, email, password_hash, role) VALUES
('Alice Johnson', 'alice@example.com', 'hash123', 'student'),
('Bob Smith', 'bob@example.com', 'hash456', 'student'),
('Dr. Emily Brown', 'emily@example.com', 'hash789', 'instructor'),
('Admin User', 'admin@example.com', 'hash000', 'admin');

INSERT INTO Courses (title, description, instructor_id, price) VALUES
('Introduction to Python', 'Learn Python basics including syntax, data types, and control structures.', 3, 49.99),
('Web Development with HTML & CSS', 'Build responsive websites using HTML and CSS.', 3, 39.99);

INSERT INTO Enrollments (user_id, course_id, status) VALUES
(1, 1, 'active'),
(2, 1, 'active'),
(1, 2, 'active');

INSERT INTO Modules (course_id, title, content, position) VALUES
(1, 'Python Basics', 'Introduction to Python, variables, and data types.', 1),
(1, 'Control Structures', 'If-else statements, loops, and logical operators.', 2),
(2, 'HTML Fundamentals', 'Learn HTML tags, structure, and attributes.', 1),
(2, 'CSS Styling', 'Introduction to CSS selectors and properties.', 2);

INSERT INTO Assignments (course_id, title, description, due_date) VALUES
(1, 'Python Assignment 1', 'Write a program to calculate factorial of a number.', '2025-09-20'),
(2, 'HTML & CSS Project', 'Create a simple portfolio website.', '2025-09-25');

INSERT INTO Submissions (assignment_id, student_id, grade, feedback) VALUES
(1, 1, 95.5, 'Great work!'),
(1, 2, 88.0, 'Good job, but missing edge cases.');

INSERT INTO Quizzes (course_id, title) VALUES
(1, 'Python Basics Quiz'),
(2, 'HTML Fundamentals Quiz');

INSERT INTO Questions (quiz_id, question_text, option_a, option_b, option_c, option_d, correct_option) VALUES
(1, 'Which of the following is a valid variable name in Python?', '2var', '_name', 'class', 'for', 'B'),
(1, 'What is the output of print(2**3)?', '6', '8', '9', '16', 'B'),
(2, 'Which HTML tag is used for inserting a line break?', '<br>', '<hr>', '<p>', '<lb>', 'A');

INSERT INTO QuizAttempts (quiz_id, student_id, score) VALUES
(1, 1, 90.0),
(1, 2, 85.0);

INSERT INTO Payments (user_id, course_id, amount, status) VALUES
(1, 1, 49.99, 'completed'),
(1, 2, 39.99, 'completed'),
(2, 1, 49.99, 'completed');
SHOW TABLES;

DESCRIBE Users;
DESCRIBE Courses;
DESCRIBE Enrollments;
DESCRIBE Modules;
DESCRIBE Assignments;
DESCRIBE Submissions;
DESCRIBE Quizzes;
DESCRIBE Questions;
DESCRIBE QuizAttempts;
DESCRIBE Payments;

SELECT * FROM Users;
SELECT * FROM Courses;
SELECT * FROM Enrollments;
SELECT * FROM Modules;
SELECT * FROM Assignments;
SELECT * FROM Submissions;
SELECT * FROM Quizzes;
SELECT * FROM Questions;
SELECT * FROM QuizAttempts;
SELECT * FROM Payments;
