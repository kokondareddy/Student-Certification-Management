# Student-Certification-Management
-- DROP SCHEMA IF EXISTS BUAN6320_008_Groups9;
-- CREATE SCHEMA IF NOT EXISTS BUAN6320_008_Groups9;
-- USE BUAN6320_008_Groups9;
DROP DATABASE IF EXISTS BUAN6320_008_Groups9;
CREATE DATABASE BUAN6320_008_Groups9;
USE BUAN6320_008_Groups9;

#--------------------------------------------- TABLE CREATION ---------------------------------------------#

-- Description: Stores information about students.
CREATE TABLE Students_T (
    StudentID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    DateOfBirth DATE,
    Address VARCHAR(50),
    Gender VARCHAR(30),
    ContactInfo VARCHAR(100)
);

-- Description: Stores information about courses offered.
CREATE TABLE Courses_T (
    CourseID INT PRIMARY KEY,
    CourseName VARCHAR(100),
    CourseDescription TEXT,
    Credits INT
);

-- Description: Stores information about instructors.
CREATE TABLE Instructors_T (
    InstructorID INT PRIMARY KEY,
    FirstName VARCHAR(50),
    LastName VARCHAR(50),
    DateOfBirth DATE,
    ContactInfo VARCHAR(100)
);

-- Description: Tracks student enrollments in courses.
CREATE TABLE Enrollments_T (
    EnrollmentID INT PRIMARY KEY,
    StudentID INT,
    CourseID INT,
    EnrollmentDate DATE,
    FOREIGN KEY (StudentID) REFERENCES Students_T(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses_T(CourseID)
);

-- Description: Stores information about certifications.
CREATE TABLE Certifications_T (
    CertificationID INT PRIMARY KEY,
    CertificationName VARCHAR(100),
    CertificationDescription TEXT
);

-- Description: Specifies course requirements for certifications.
CREATE TABLE CertificationRequirements_T (
    RequirementID INT PRIMARY KEY,
    CertificationID INT,
    CourseID INT,
    FOREIGN KEY (CertificationID) REFERENCES Certifications_T(CertificationID),
    FOREIGN KEY (CourseID) REFERENCES Courses_T(CourseID)
);

-- Description: Records student applications for certifications.
CREATE TABLE CertificationApplications_T (
    ApplicationID INT PRIMARY KEY,
    StudentID INT,
    CertificationID INT,
    ApplicationDate DATE,
    ApplicationStatus VARCHAR(20),
    FOREIGN KEY (StudentID) REFERENCES Students_T(StudentID),
    FOREIGN KEY (CertificationID) REFERENCES Certifications_T(CertificationID)
);

-- Description: Defines user roles within the system.
CREATE TABLE Roles_T (
    RoleID INT PRIMARY KEY,
    RoleName VARCHAR(50),
    RoleDescription TEXT
);

-- Description: Manages user accounts for the system.
CREATE TABLE Users_T (
    UserID INT PRIMARY KEY,
    Username VARCHAR(50),
    PasswordHash VARCHAR(100),
    Email VARCHAR(100),
    RoleID INT,
    FOREIGN KEY (RoleID) REFERENCES Roles_T(RoleID)
);

-- Description: Tracks the approval status of certification applications.
DROP TABLE IF EXISTS CertificationApprovals_T;
CREATE TABLE CertificationApprovals_T (
    ApprovalID INT PRIMARY KEY,
    ApplicationID INT,
    ApproverID INT,
    ApprovalDate DATE,
    ApprovalStatus VARCHAR(20),
    FOREIGN KEY (ApplicationID) REFERENCES CertificationApplications_T(ApplicationID),
    FOREIGN KEY (ApproverID) REFERENCES Users_T(UserID)
);

-- Description: Stores student course grades.
CREATE TABLE Transcript_T (
    TranscriptID INT PRIMARY KEY,
    StudentID INT,
    CourseID INT,
    Grade VARCHAR(2),
    Semester VARCHAR(20),
    FOREIGN KEY (StudentID) REFERENCES Students_T(StudentID),
    FOREIGN KEY (CourseID) REFERENCES Courses_T(CourseID)
);

-- Description: Records payment details for courses or certifications.
CREATE TABLE Payments_T (
    PaymentID INT PRIMARY KEY,
    StudentID INT,
    Amount DECIMAL(10, 2),
    PaymentDate DATE,
    PaymentMethod VARCHAR(50),
    FOREIGN KEY (StudentID) REFERENCES Students_T(StudentID)
);

-- Description: Stores information about academic departments.
CREATE TABLE Departments_T (
    DepartmentID INT PRIMARY KEY,
    DepartmentName VARCHAR(100),
    DepartmentDescription TEXT
);

-- Description: Defines instructor roles within departments.
CREATE TABLE FacultyDepartments_T (
    FacultyDepartmentID INT PRIMARY KEY,
    InstructorID INT,
    DepartmentID INT,
    Roles VARCHAR(50),
    FOREIGN KEY (InstructorID) REFERENCES Instructors_T(InstructorID),
    FOREIGN KEY (DepartmentID) REFERENCES Departments_T(DepartmentID)
);

-- Description: Records events related to certifications or courses.
CREATE TABLE Events_T (
    EventID INT PRIMARY KEY,
    EventName VARCHAR(100),
    EventDescription TEXT,
    EventDate DATE,
    Location VARCHAR(100)
);

-- Description: Tracks student registrations for events.
CREATE TABLE EventRegistrations_T (
    RegistrationID INT PRIMARY KEY,
    EventID INT,
    StudentID INT,
    RegistrationDate DATE,
    CertificationStatus VARCHAR(20),
    FOREIGN KEY (EventID) REFERENCES Events_T(EventID),
    FOREIGN KEY (StudentID) REFERENCES Students_T(StudentID)
);

-- Description: Stores notifications sent to students.
CREATE TABLE Notifications_T (
    NotificationID INT PRIMARY KEY,
    StudentID INT,
    Message TEXT,
    NotificationTimestamp TIMESTAMP,
    CertificationStatus VARCHAR(20),
    FOREIGN KEY (StudentID) REFERENCES Students_T(StudentID)
);

-- Description: Specifies permissions associated with roles.
CREATE TABLE Permissions_T (
    PermissionID INT PRIMARY KEY,
    PermissionName VARCHAR(50),
    PermissionDescription TEXT
);

-- Description: Maps roles to their associated permissions.
CREATE TABLE RolePermissions_T (
    RolePermissionID INT PRIMARY KEY,
    RoleID INT,
    PermissionID INT,
    FOREIGN KEY (RoleID) REFERENCES Roles_T(RoleID),
    FOREIGN KEY (PermissionID) REFERENCES Permissions_T(PermissionID)
);

-- Description: Records system activity for auditing purposes.
CREATE TABLE AuditLogs_T (
    LogID INT PRIMARY KEY,
    UserID INT,
    ActionTaken VARCHAR(100),
    AuditTimestamp TIMESTAMP,
    IPAddress VARCHAR(50),
    FOREIGN KEY (UserID) REFERENCES Users_T(UserID)
);



#--------------------------------------------- DATA INSERTION ---------------------------------------------#
 

select * from Students_T;
INSERT INTO Students_T (StudentID, FirstName, LastName, DateOfBirth, Address, Gender, ContactInfo)
VALUES
    (1, 'John', 'Doe', '1995-03-15', '123 Main St', 'Male', 'johndoe@email.com'),
    (2, 'Jane', 'Smith', '1996-07-20', '456 Elm St', 'Female', 'janesmith@email.com'),
    (3, 'Alice', 'Johnson', '1998-11-05', '789 Oak St', 'Female', 'alice@email.com'),
    (4, 'Bob', 'Williams', '1997-08-10', '567 Pine St', 'Male', 'bob@email.com'),
    (5, 'Eva', 'Brown', '1994-02-25', '456 Birch St', 'Female', 'eva@email.com'),
    (6, 'David', 'Lee', '1999-04-30', '234 Maple St', 'Male', 'david@email.com'),
    (7, 'Olivia', 'Wilson', '1996-09-12', '789 Oak St', 'Female', 'olivia@email.com'),
    (8, 'William', 'Clark', '1997-12-02', '567 Pine St', 'Male', 'william@email.com'),
    (9, 'Sophia', 'Garcia', '1998-10-08', '456 Birch St', 'Female', 'sophia@email.com'),
    (10, 'James', 'Martinez', '1995-06-20', '234 Maple St', 'Male', 'james@email.com'),
    (11, 'Lily', 'Anderson', '1996-03-18', '123 Main St', 'Female', 'lily@email.com'),
    (12, 'Samuel', 'Hall', '1998-07-15', '456 Elm St', 'Male', 'samuel@email.com'),
    (13, 'Ava', 'Turner', '1999-01-25', '789 Oak St', 'Female', 'ava@email.com'),
    (14, 'Daniel', 'Harris', '1997-04-05', '567 Pine St', 'Male', 'daniel@email.com'),
    (15, 'Mia', 'Perez', '1994-11-10', '456 Birch St', 'Female', 'mia@email.com'),
    (16, 'Joseph', 'Young', '1995-02-28', '234 Maple St', 'Male', 'joseph@email.com'),
    (17, 'Emily', 'Brown', '1996-10-17', '123 Main St', 'Female', 'emily@email.com'),
    (18, 'Alexander', 'King', '1997-05-15', '456 Elm St', 'Male', 'alexander@email.com'),
    (19, 'Grace', 'Rodriguez', '1998-12-03', '789 Oak St', 'Female', 'grace@email.com'),
    (20, 'Michael', 'Scott', '1995-08-22', '567 Pine St', 'Male', 'michael@email.com');
    
    
select * from Courses_T;

INSERT INTO Courses_T (CourseID, CourseName, CourseDescription, Credits)
VALUES
    (1, 'Introduction to Programming', 'A beginner-level course on programming fundamentals.', 3),
    (2, 'Database Management', 'Learn about database design, SQL, and database administration.', 4),
    (3, 'Web Development Fundamentals', 'Explore web technologies like HTML, CSS, and JavaScript.', 3),
    (4, 'Data Structures and Algorithms', 'Advanced study of data structures and algorithmic analysis.', 4),
    (5, 'Software Engineering Principles', 'Focus on software development methodologies and best practices.', 3),
    (6, 'Artificial Intelligence', 'An introduction to AI concepts and techniques.', 4),
    (7, 'Networking and Security', 'Study of computer networks and cybersecurity.', 3),
    (8, 'Distributed Systems', 'Explore the design and implementation of distributed systems.', 4),
    (9, 'Mobile App Development', 'Development of mobile applications for iOS and Android.', 3),
    (10, 'Machine Learning', 'Advanced machine learning and data analysis.', 4),
    (11, 'Human-Computer Interaction', 'Study of user interface design and usability.', 3),
    (12, 'Software Testing and Quality Assurance', 'Focus on testing methodologies and QA processes.', 3),
    (13, 'Computer Graphics', 'Exploration of 2D and 3D computer graphics techniques.', 4),
    (14, 'Operating Systems', 'Study of operating system principles and design.', 4),
    (15, 'Cloud Computing', 'Introduction to cloud technologies and services.', 3),
    (16, 'Digital Marketing', 'Marketing strategies in the digital age.', 3),
    (17, 'Data Science and Analytics', 'Data analysis and data-driven decision making.', 4),
    (18, 'Blockchain and Cryptocurrency', 'Introduction to blockchain and cryptocurrencies.', 3),
    (19, 'E-commerce and Online Business', 'Strategies for running online businesses.', 3),
    (20, 'Game Development', 'Development of video games for various platforms.', 4);
    
select * from Instructors_T;

INSERT INTO Instructors_T (InstructorID, FirstName, LastName, DateOfBirth, ContactInfo)
VALUES
    (1, 'Sarah', 'Johnson', '1980-01-10', 'sarah@email.com'),
    (2, 'Michael', 'Brown', '1975-05-22', 'michael@email.com'),
    (3, 'Jennifer', 'Smith', '1983-09-15', 'jennifer@email.com'),
    (4, 'David', 'Wilson', '1978-12-04', 'david@email.com'),
    (5, 'Jessica', 'Lee', '1982-06-20', 'jessica@email.com'),
    (6, 'Robert', 'Martin', '1979-03-25', 'robert@email.com'),
    (7, 'Amanda', 'Anderson', '1985-02-15', 'amanda@email.com'),
    (8, 'John', 'Taylor', '1976-08-10', 'john@email.com'),
    (9, 'Melissa', 'Harris', '1984-04-30', 'melissa@email.com'),
    (10, 'Christopher', 'Moore', '1981-11-18', 'christopher@email.com'),
    (11, 'Emily', 'Martinez', '1983-07-22', 'emily@email.com'),
    (12, 'William', 'Thomas', '1977-06-05', 'william@email.com'),
    (13, 'Linda', 'Garcia', '1979-10-08', 'linda@email.com'),
    (14, 'Brian', 'Miller', '1980-04-15', 'brian@email.com'),
    (15, 'Karen', 'White', '1978-03-28', 'karen@email.com'),
    (16, 'Richard', 'Clark', '1982-02-10', 'richard@email.com'),
    (17, 'Susan', 'Davis', '1985-09-17', 'susan@email.com'),
    (18, 'Daniel', 'Jackson', '1986-05-20', 'daniel@email.com'),
    (19, 'Patricia', 'Adams', '1977-12-03', 'patricia@email.com'),
    (20, 'Mark', 'Brown', '1981-08-22', 'mark@email.com');
    
    
select * from Enrollments_T;

INSERT INTO Enrollments_T (EnrollmentID, StudentID, CourseID, EnrollmentDate)
VALUES
    (1, 1, 1, '2023-01-15'),
    (2, 2, 2, '2023-02-20'),
    (3, 3, 3, '2023-03-10'),
    (4, 4, 4, '2023-04-05'),
    (5, 5, 5, '2023-05-15'),
    (6, 6, 6, '2023-06-20'),
    (7, 7, 7, '2023-07-02'),
    (8, 8, 8, '2023-08-12'),
    (9, 9, 9, '2023-09-25'),
    (10, 10, 10, '2023-10-18'),
    (11, 11, 11, '2023-11-07'),
    (12, 12, 12, '2023-12-22'),
    (13, 13, 13, '2024-01-30'),
    (14, 14, 14, '2024-02-11'),
    (15, 15, 15, '2024-03-14'),
    (16, 16, 16, '2024-04-09'),
    (17, 17, 17, '2024-05-23'),
    (18, 18, 18, '2024-06-08'),
    (19, 19, 19, '2024-07-05'),
    (20, 20, 20, '2024-08-17');


select * from Certifications_T;

INSERT INTO Certifications_T (CertificationID, CertificationName, CertificationDescription)
VALUES
    (1, 'Certified Web Developer', 'This certification validates expertise in web development.'),
    (2, 'Certified Data Analyst', 'Become proficient in data analysis techniques.'),
    (3, 'Certified Network Administrator', 'Master the art of network administration.'),
    (4, 'Certified Java Programmer', 'Demonstrate proficiency in Java programming.'),
    (5, 'Certified Project Manager', 'Learn effective project management techniques.'),
    (6, 'Certified Cloud Architect', 'Become skilled in cloud infrastructure design.'),
    (7, 'Certified Cybersecurity Specialist', 'Enhance your cybersecurity skills.'),
    (8, 'Certified Machine Learning Engineer', 'Master machine learning and AI.'),
    (9, 'Certified Digital Marketing Professional', 'Excel in the digital marketing landscape.'),
    (10, 'Certified Data Scientist', 'Master the science of data analysis.'),
    (11, 'Certified Business Analyst', 'Become a proficient business analyst.'),
    (12, 'Certified Software Tester', 'Excel in software testing and QA.'),
    (13, 'Certified Graphics Designer', 'Learn the art of graphic design.'),
    (14, 'Certified DevOps Engineer', 'Master DevOps practices and tools.'),
    (15, 'Certified System Administrator', 'Excel in system administration tasks.'),
    (16, 'Certified Database Administrator', 'Learn advanced database management.'),
    (17, 'Certified AI Ethics Consultant', 'Master AI ethics and compliance.'),
    (18, 'Certified Blockchain Developer', 'Learn to develop blockchain solutions.'),
    (19, 'Certified UX/UI Designer', 'Excel in user experience and user interface design.'),
    (20, 'Certified Scrum Master', 'Become a certified Scrum master.');
    

select * from CertificationRequirements_T;

INSERT INTO CertificationRequirements_T (RequirementID, CertificationID, CourseID)
VALUES
    (1, 1, 1),  -- Certified Web Developer requires Introduction to Programming
    (2, 1, 3),  -- Certified Web Developer requires Web Development Fundamentals
    (3, 1, 5),  -- Certified Web Developer requires Software Engineering Principles
    (4, 2, 2),  -- Certified Data Analyst requires Database Management
    (5, 2, 4),  -- Certified Data Analyst requires Data Structures and Algorithms
    (6, 2, 17), -- Certified Data Analyst requires Data Science and Analytics
    (7, 3, 7),  -- Certified Network Administrator requires Networking and Security
    (8, 3, 8),  -- Certified Network Administrator requires Distributed Systems
    (9, 4, 1),  -- Certified Java Programmer requires Introduction to Programming
    (10, 4, 13), -- Certified Java Programmer requires Computer Graphics
    (11, 5, 5), -- Certified Project Manager requires Software Engineering Principles
    (12, 5, 12), -- Certified Project Manager requires Software Testing and Quality Assurance
    (13, 6, 6), -- Certified Cloud Architect requires Artificial Intelligence
    (14, 6, 8), -- Certified Cloud Architect requires Distributed Systems
    (15, 7, 3), -- Certified Cybersecurity Specialist requires Web Development Fundamentals
    (16, 7, 7), -- Certified Cybersecurity Specialist requires Networking and Security
    (17, 8, 10), -- Certified Machine Learning Engineer requires Machine Learning
    (18, 8, 17), -- Certified Machine Learning Engineer requires Data Science and Analytics
    (19, 9, 9),  -- Certified Digital Marketing Professional requires Mobile App Development
    (20, 9, 16); -- Certified Digital Marketing Professional requires Digital Marketing
    
select * from CertificationApplications_T;

INSERT INTO CertificationApplications_T (ApplicationID, StudentID, CertificationID, ApplicationDate, ApplicationStatus)
VALUES
    (1, 1, 1, '2023-01-15', 'Pending'),
    (2, 2, 2, '2023-02-20', 'Approved'),
    (3, 3, 3, '2023-03-10', 'Rejected'),
    (4, 4, 1, '2023-04-05', 'Pending'),
    (5, 5, 2, '2023-05-15', 'Pending'),
    (6, 6, 3, '2023-06-20', 'Approved'),
    (7, 7, 1, '2023-07-02', 'Pending'),
    (8, 8, 2, '2023-08-12', 'Approved'),
    (9, 9, 3, '2023-09-25', 'Rejected'),
    (10, 10, 1, '2023-10-18', 'Approved'),
    (11, 11, 2, '2023-11-07', 'Pending'),
    (12, 12, 3, '2023-12-22', 'Rejected'),
    (13, 13, 1, '2024-01-30', 'Pending'),
    (14, 14, 2, '2024-02-11', 'Approved'),
    (15, 15, 3, '2024-03-14', 'Pending'),
    (16, 16, 1, '2024-04-09', 'Rejected'),
    (17, 17, 2, '2024-05-23', 'Approved'),
    (18, 18, 3, '2024-06-08', 'Pending'),
    (19, 19, 1, '2024-07-05', 'Approved'),
    (20, 20, 2, '2024-08-17', 'Pending');
    
select * from Roles_T;

INSERT INTO Roles_T (RoleID, RoleName, RoleDescription)
VALUES
    (1, 'Admin', 'System administrator with full access and control.'),
    (2, 'Instructor', 'Teaching and course management role.'),
    (3, 'Student', 'Enrolled in courses and certifications.'),
    (4, 'Manager', 'Management and supervisory role.'),
    (5, 'Support Staff', 'Provides support and assistance to users.'),
    (6, 'Guest', 'Limited access for visitors and guests.'),
    (7, 'Editor', 'Role with editing and content management permissions.'),
    (8, 'Analyst', 'Analyzes data and generates reports.'),
    (9, 'Developer', 'Software and application development role.'),
    (10, 'Researcher', 'Conducts research and publishes findings.'),
    (11, 'Marketing Specialist', 'Handles marketing and promotional activities.'),
    (12, 'Designer', 'Graphic and web design role.'),
    (13, 'HR Manager', 'Human resources management role.'),
    (14, 'Financial Analyst', 'Analyzes financial data and reports.'),
    (15, 'Customer Support', 'Provides customer support and assistance.'),
    (16, 'Security Officer', 'Ensures system security and compliance.'),
    (17, 'Data Scientist', 'Analyzes and interprets data for insights.'),
    (18, 'Event Coordinator', 'Coordinates and manages events and activities.'),
    (19, 'Librarian', 'Manages library resources and services.'),
    (20, 'Academic Advisor', 'Provides academic guidance and counseling.');


select * from Users_T;

INSERT INTO Users_T (UserID, Username, PasswordHash, Email, RoleID)
VALUES
    (1, 'admin_user', 'hashed_password_1', 'admin@example.com', 1),
    (2, 'instructor_1', 'hashed_password_2', 'instructor1@example.com', 2),
    (3, 'student_1', 'hashed_password_3', 'student1@example.com', 3),
    (4, 'manager_1', 'hashed_password_4', 'manager1@example.com', 4),
    (5, 'support_staff_1', 'hashed_password_5', 'support1@example.com', 5),
    (6, 'guest_user', 'hashed_password_6', 'guest@example.com', 6),
    (7, 'editor_1', 'hashed_password_7', 'editor1@example.com', 7),
    (8, 'analyst_1', 'hashed_password_8', 'analyst1@example.com', 8),
    (9, 'developer_1', 'hashed_password_9', 'developer1@example.com', 9),
    (10, 'researcher_1', 'hashed_password_10', 'researcher1@example.com', 10),
    (11, 'marketing_specialist_1', 'hashed_password_11', 'marketing1@example.com', 11),
    (12, 'designer_1', 'hashed_password_12', 'designer1@example.com', 12),
    (13, 'hr_manager_1', 'hashed_password_13', 'hr1@example.com', 13),
    (14, 'financial_analyst_1', 'hashed_password_14', 'finance1@example.com', 14),
    (15, 'customer_support_1', 'hashed_password_15', 'support2@example.com', 15),
    (16, 'security_officer_1', 'hashed_password_16', 'security1@example.com', 16),
    (17, 'data_scientist_1', 'hashed_password_17', 'scientist1@example.com', 17),
    (18, 'event_coordinator_1', 'hashed_password_18', 'events1@example.com', 18),
    (19, 'librarian_1', 'hashed_password_19', 'librarian1@example.com', 19),
    (20, 'academic_advisor_1', 'hashed_password_20', 'advisor1@example.com', 20);


select * from CertificationApprovals_T;

INSERT INTO CertificationApprovals_T (ApprovalID, ApplicationID, ApproverID, ApprovalDate, ApprovalStatus)
VALUES
    (1, 1, 1, '2023-01-15', 'Approved'),
    (2, 2, 2, '2023-02-20', 'Approved'),
    (3, 3, 3, '2023-03-10', 'Pending'),
    (4, 4, 4, '2023-04-05', 'Approved'),
    (5, 5, 5, '2023-05-15', 'Rejected'),
    (6, 6, 6, '2023-06-20', 'Approved'),
    (7, 7, 7, '2023-07-02', 'Pending'),
    (8, 8, 8, '2023-08-12', 'Approved'),
    (9, 9, 9, '2023-09-25', 'Approved'),
    (10, 10, 10, '2023-10-18', 'Pending'),
    (11, 11, 11, '2023-11-07', 'Rejected'),
    (12, 12, 12, '2023-12-22', 'Approved'),
    (13, 13, 13, '2024-01-30', 'Approved'),
    (14, 14, 14, '2024-02-11', 'Rejected'),
    (15, 15, 15, '2024-03-14', 'Approved'),
    (16, 16, 16, '2024-04-09', 'Pending'),
    (17, 17, 17, '2024-05-23', 'Approved'),
    (18, 18, 18, '2024-06-08', 'Approved'),
    (19, 19, 19, '2024-07-05', 'Rejected'),
    (20, 20, 20, '2024-08-17', 'Approved');


select * from Transcript_T;

INSERT INTO Transcript_T (TranscriptID, StudentID, CourseID, Grade, Semester)
VALUES
    (1, 1, 1, 'A', 'Spring 2023'),
    (2, 1, 2, 'B', 'Spring 2023'),
    (3, 1, 3, 'A', 'Summer 2023'),
    (4, 2, 2, 'A', 'Spring 2023'),
    (5, 2, 4, 'B', 'Summer 2023'),
    (6, 2, 6, 'A', 'Fall 2023'),
    (7, 3, 1, 'B', 'Spring 2023'),
    (8, 3, 3, 'C', 'Summer 2023'),
    (9, 3, 5, 'A', 'Fall 2023'),
    (10, 4, 4, 'A', 'Spring 2023'),
    (11, 4, 6, 'B', 'Summer 2023'),
    (12, 4, 8, 'A', 'Fall 2023'),
    (13, 5, 2, 'A', 'Spring 2023'),
    (14, 5, 7, 'B', 'Summer 2023'),
    (15, 5, 9, 'A', 'Fall 2023'),
    (16, 6, 3, 'B', 'Spring 2023'),
    (17, 6, 5, 'C', 'Summer 2023'),
    (18, 6, 7, 'A', 'Fall 2023'),
    (19, 7, 6, 'A', 'Spring 2023'),
    (20, 7, 8, 'B', 'Summer 2023');

select * from Payments_T;

INSERT INTO Payments_T (PaymentID, StudentID, Amount, PaymentDate, PaymentMethod)
VALUES
    (1, 1, 200.00, '2023-01-15', 'Credit Card'),
    (2, 1, 50.00, '2023-02-20', 'PayPal'),
    (3, 2, 180.00, '2023-03-10', 'Credit Card'),
    (4, 2, 75.00, '2023-04-05', 'Bank Transfer'),
    (5, 3, 250.00, '2023-05-15', 'Credit Card'),
    (6, 3, 60.00, '2023-06-20', 'PayPal'),
    (7, 4, 210.00, '2023-07-02', 'Credit Card'),
    (8, 4, 80.00, '2023-08-12', 'Bank Transfer'),
    (9, 5, 230.00, '2023-09-25', 'Credit Card'),
    (10, 5, 70.00, '2023-10-18', 'PayPal'),
    (11, 6, 195.00, '2023-11-07', 'Credit Card'),
    (12, 6, 65.00, '2023-12-22', 'Bank Transfer'),
    (13, 7, 220.00, '2024-01-30', 'Credit Card'),
    (14, 7, 55.00, '2024-02-11', 'PayPal'),
    (15, 8, 200.00, '2024-03-14', 'Credit Card'),
    (16, 8, 75.00, '2024-04-09', 'Bank Transfer'),
    (17, 9, 240.00, '2024-05-23', 'Credit Card'),
    (18, 9, 70.00, '2024-06-08', 'PayPal'),
    (19, 10, 210.00, '2024-07-05', 'Credit Card'),
    (20, 10, 60.00, '2024-08-17', 'Bank Transfer');

select * from Departments_T;

INSERT INTO Departments_T (DepartmentID, DepartmentName, DepartmentDescription)
VALUES
    (1, 'Computer Science Department', 'This department focuses on computer science and related fields.'),
    (2, 'Engineering Department', 'The engineering department covers a wide range of engineering disciplines.'),
    (3, 'Business School', 'This department specializes in business and management programs.'),
    (4, 'Mathematics Department', 'The mathematics department offers a variety of math courses.'),
    (5, 'Social Sciences Department', 'This department covers a range of social science fields.'),
    (6, 'Art and Design Department', 'The department of art and design offers various art courses.'),
    (7, 'Science Department', 'This department encompasses various scientific disciplines.'),
    (8, 'Language and Literature Department', 'Language and literature courses are offered in this department.'),
    (9, 'Health Sciences Department', 'This department focuses on health-related programs.'),
    (10, 'Education Department', 'The education department offers teaching and educational programs.'),
    (11, 'Music Department', 'Various music courses are available in this department.'),
    (12, 'History Department', 'This department covers historical studies.'),
    (13, 'Psychology Department', 'Psychology programs and courses are offered here.'),
    (14, 'Environmental Science Department', 'Focuses on environmental science and sustainability.'),
    (15, 'Economics Department', 'Economics and financial courses are available here.'),
    (16, 'Communication Department', 'Communication studies and programs are provided in this department.'),
    (17, 'Physics Department', 'Physics courses and research are offered in this department.'),
    (18, 'Geology Department', 'This department specializes in geology and earth sciences.'),
    (19, 'Political Science Department', 'Focuses on political science and government studies.'),
    (20, 'Philosophy Department', 'Philosophy courses and studies are available in this department.');

select * from FacultyDepartments_T;

INSERT INTO FacultyDepartments_T (FacultyDepartmentID, InstructorID, DepartmentID, Roles)
VALUES
    (1, 1, 1, 'Professor'),
    (2, 2, 2, 'Associate Professor'),
    (3, 3, 3, 'Professor'),
    (4, 4, 4, 'Assistant Professor'),
    (5, 5, 5, 'Professor'),
    (6, 6, 6, 'Associate Professor'),
    (7, 7, 7, 'Professor'),
    (8, 8, 8, 'Assistant Professor'),
    (9, 9, 9, 'Professor'),
    (10, 10, 10, 'Associate Professor'),
    (11, 11, 11, 'Professor'),
    (12, 12, 12, 'Assistant Professor'),
    (13, 13, 13, 'Professor'),
    (14, 14, 14, 'Associate Professor'),
    (15, 15, 15, 'Professor'),
    (16, 16, 16, 'Assistant Professor'),
    (17, 17, 17, 'Professor'),
    (18, 18, 18, 'Associate Professor'),
    (19, 19, 19, 'Professor'),
    (20, 20, 20, 'Assistant Professor');

select * from Events_T;

INSERT INTO Events_T (EventID, EventName, EventDescription, EventDate, Location)
VALUES
    (1, 'Certification Workshop', 'Workshop to prepare for certification exams.', '2023-02-15', 'Conference Center'),
    (2, 'Career Fair', 'An event to connect students with job opportunities.', '2023-03-20', 'Campus Expo Hall'),
    (3, 'Web Development Seminar', 'Seminar on the latest web development trends.', '2023-04-10', 'Room 101'),
    (4, 'Data Science Conference', 'A conference on data science and analytics.', '2023-05-05', 'Auditorium A'),
    (5, 'Programming Competition', 'Annual programming competition.', '2023-06-15', 'Computer Labs'),
    (6, 'AI and Robotics Expo', 'Exhibition of AI and robotics projects.', '2023-07-20', 'Expo Center'),
    (7, 'Cybersecurity Symposium', 'Symposium on the latest cybersecurity threats.', '2023-08-02', 'Seminar Hall'),
    (8, 'Machine Learning Workshop', 'Workshop on machine learning techniques.', '2023-09-12', 'Lab 3A'),
    (9, 'Digital Marketing Conference', 'Conference on digital marketing strategies.', '2023-10-25', 'Meeting Room B'),
    (10, 'Data Analysis Seminar', 'Seminar on data analysis methodologies.', '2023-11-18', 'Seminar Room 2'),
    (11, 'Software Testing Workshop', 'Workshop on software testing best practices.', '2023-12-20', 'Lab 4B'),
    (12, 'Computer Graphics Exhibition', 'Exhibition of computer graphics projects.', '2024-01-30', 'Gallery Hall'),
    (13, 'Operating Systems Lecture', 'Guest lecture on operating systems.', '2024-02-15', 'Lecture Hall A'),
    (14, 'Cloud Computing Conference', 'Conference on cloud computing technologies.', '2024-03-10', 'Conference Center'),
    (15, 'E-commerce Workshop', 'Workshop on e-commerce and online business.', '2024-04-05', 'Lab 2C'),
    (16, 'Game Development Expo', 'Exhibition of student game development projects.', '2024-05-22', 'Game Studio'),
    (17, 'Blockchain Seminar', 'Seminar on blockchain technology and applications.', '2024-06-20', 'Meeting Room C'),
    (18, 'UX/UI Design Workshop', 'Workshop on user experience and interface design.', '2024-07-15', 'Design Lab'),
    (19, 'Scrum Master Training', 'Training for Scrum Master certification.', '2024-08-12', 'Training Room 1'),
    (20, 'Art and Design Exhibition', 'Exhibition of student artwork and design projects.', '2024-09-05', 'Art Gallery');

select * from EventRegistrations_T;

INSERT INTO EventRegistrations_T (RegistrationID, EventID, StudentID, RegistrationDate, CertificationStatus)
VALUES
    (1, 1, 1, '2023-01-15', 'Registered'),
    (2, 2, 2, '2023-02-20', 'Registered'),
    (3, 3, 3, '2023-03-10', 'Attended'),
    (4, 4, 4, '2023-04-05', 'Registered'),
    (5, 5, 5, '2023-05-15', 'Attended'),
    (6, 6, 6, '2023-06-20', 'Registered'),
    (7, 7, 7, '2023-07-02', 'Registered'),
    (8, 8, 8, '2023-08-12', 'Attended'),
    (9, 9, 9, '2023-09-25', 'Registered'),
    (10, 10, 10, '2023-10-18', 'Registered'),
    (11, 11, 11, '2023-11-07', 'Attended'),
    (12, 12, 12, '2023-12-22', 'Registered'),
    (13, 13, 13, '2024-01-30', 'Registered'),
    (14, 14, 14, '2024-02-11', 'Attended'),
    (15, 15, 15, '2024-03-14', 'Registered'),
    (16, 16, 16, '2024-04-09', 'Registered'),
    (17, 17, 17, '2024-05-23', 'Registered'),
    (18, 18, 18, '2024-06-08', 'Attended'),
    (19, 19, 19, '2024-07-05', 'Registered'),
    (20, 20, 20, '2024-08-17', 'Registered');


select * from Notifications_T;

INSERT INTO Notifications_T (NotificationID, StudentID, Message, NotificationTimestamp, CertificationStatus)
VALUES
    (1, 1, 'Congratulations! Your certification application has been approved.', '2023-01-15 10:30:00', 'Approved'),
    (2, 2, 'Reminder: Your certification workshop is scheduled for tomorrow.', '2023-02-20 15:45:00', 'Pending'),
    (3, 3, 'New course materials for your certification are available.', '2023-03-10 11:15:00', 'Approved'),
    (4, 4, 'Payment received for your certification application.', '2023-04-05 08:00:00', 'Approved'),
    (5, 5, 'Your certification application has been rejected. Please check the details.', '2023-05-15 16:20:00', 'Rejected'),
    (6, 6, 'Reminder: Your event registration is confirmed for next week.', '2023-06-20 14:30:00', 'Registered'),
    (7, 7, 'New course added to your program. Check the updated curriculum.', '2023-07-02 09:45:00', 'Approved'),
    (8, 8, 'Payment due for your course. Please make a payment at your earliest convenience.', '2023-08-12 13:10:00', 'Pending'),
    (9, 9, 'Your event registration has been confirmed. Prepare for an exciting event!', '2023-09-25 11:55:00', 'Registered'),
    (10, 10, 'Reminder: Your certification exam is scheduled for next month.', '2023-10-18 17:00:00', 'Pending'),
    (11, 11, 'Notification: Important updates to your certification requirements.', '2023-11-07 10:30:00', 'Approved'),
    (12, 12, 'Payment received for your course. Thank you for your payment.', '2023-12-22 08:45:00', 'Approved'),
    (13, 13, 'Notification: Changes to the event schedule. Please check the new dates.', '2024-01-30 14:00:00', 'Registered'),
    (14, 14, 'Reminder: Your event registration is confirmed for next week.', '2024-02-11 16:15:00', 'Registered'),
    (15, 15, 'Your certification application has been approved. Congratulations!', '2024-03-14 09:30:00', 'Approved'),
    (16, 16, 'Reminder: Your certification workshop is scheduled for next week.', '2024-04-09 13:50:00', 'Pending'),
    (17, 17, 'Notification: New course materials for your program are available online.', '2024-05-23 15:25:00', 'Approved'),
    (18, 18, 'Payment due for your certification application. Please make a payment.', '2024-06-08 10:05:00', 'Pending'),
    (19, 19, 'Your event registration has been confirmed. Prepare for an exciting event!', '2024-07-05 12:40:00', 'Registered'),
    (20, 20, 'Reminder: Your event registration is confirmed for next week. Get ready!', '2024-08-17 08:20:00', 'Registered');



select * from Permissions_T;

INSERT INTO Permissions_T (PermissionID, PermissionName, PermissionDescription)
VALUES
    (1, 'ManageUsers', 'Permission to manage user accounts.'),
    (2, 'ManageCourses', 'Permission to manage courses and curriculum.'),
    (3, 'ManageCertifications', 'Permission to manage certifications and requirements.'),
    (4, 'ViewGrades', 'Permission to view student grades and transcripts.'),
    (5, 'ManageEvents', 'Permission to create and manage events and activities.'),
    (6, 'EditContent', 'Permission to edit and manage content on the platform.'),
    (7, 'GenerateReports', 'Permission to generate reports and analytics.'),
    (8, 'ManagePayments', 'Permission to manage payments and financial transactions.'),
    (9, 'AccessLibrary', 'Permission to access and manage library resources.'),
    (10, 'SecurityMonitoring', 'Permission to monitor system security and compliance.'),
    (11, 'AdvisoryServices', 'Permission to provide academic advising and counseling.'),
    (12, 'ManageFaculty', 'Permission to manage instructors and faculty departments.'),
    (13, 'ManageSupport', 'Permission to provide support and assistance to users.'),
    (14, 'MarketingActivities', 'Permission to manage marketing and promotional activities.'),
    (15, 'ArtDesignManagement', 'Permission to manage art and design resources and projects.'),
    (16, 'AccessHRData', 'Permission to access and manage human resources data.'),
    (17, 'FinancialAnalysis', 'Permission to analyze financial data and make financial decisions.'),
    (18, 'CustomerSupport', 'Permission to provide customer support and assistance.'),
    (19, 'DataAnalysis', 'Permission to analyze data for insights and decision-making.'),
    (20, 'ManageRegistrations', 'Permission to manage event and course registrations.');


select * from RolePermissions_T;

INSERT INTO RolePermissions_T (RolePermissionID, RoleID, PermissionID)
VALUES
    (1, 1, 1),
    (2, 2, 2),
    (3, 3, 3),
    (4, 4, 4),
    (5, 5, 5),
    (6, 6, 6),
    (7, 7, 7),
    (8, 8, 8),
    (9, 9, 9),
    (10, 10, 10),
    (11, 11, 11),
    (12, 12, 12),
    (13, 13, 13),
    (14, 14, 14),
    (15, 15, 15),
    (16, 16, 16),
    (17, 17, 17),
    (18, 18, 18),
    (19, 19, 19),
    (20, 20, 20);


select * from AuditLogs_T;

INSERT INTO AuditLogs_T (LogID, UserID, ActionTaken, AuditTimestamp, IPAddress)
VALUES
    (1, 1, 'User login', '2023-01-15 10:30:00', '192.168.1.1'),
    (2, 2, 'Course creation', '2023-02-20 15:45:00', '192.168.1.2'),
    (3, 3, 'Certification approval', '2023-03-10 11:15:00', '192.168.1.3'),
    (4, 4, 'Payment received', '2023-04-05 08:00:00', '192.168.1.4'),
    (5, 5, 'User logout', '2023-05-15 16:20:00', '192.168.1.5'),
    (6, 6, 'Event registration', '2023-06-20 14:30:00', '192.168.1.6'),
    (7, 7, 'Content update', '2023-07-02 09:45:00', '192.168.1.7'),
    (8, 8, 'Payment due notification', '2023-08-12 13:10:00', '192.168.1.8'),
    (9, 9, 'User login', '2023-09-25 11:55:00', '192.168.1.9'),
    (10, 10, 'Event cancellation', '2023-10-18 17:00:00', '192.168.1.10'),
    (11, 11, 'Certification rejection', '2023-11-07 10:30:00', '192.168.1.11'),
    (12, 12, 'Payment received', '2023-12-22 08:45:00', '192.168.1.12'),
    (13, 13, 'Course enrollment', '2024-01-30 14:00:00', '192.168.1.13'),
    (14, 14, 'User login', '2024-02-11 16:15:00', '192.168.1.14'),
    (15, 15, 'Certification approval', '2024-03-14 09:30:00', '192.168.1.15'),
    (16, 16, 'Event creation', '2024-04-09 13:50:00', '192.168.1.16'),
    (17, 17, 'Content update', '2024-05-23 15:25:00', '192.168.1.17'),
    (18, 18, 'Payment due notification', '2024-06-08 10:05:00', '192.168.1.18'),
    (19, 19, 'User login', '2024-07-05 12:40:00', '192.168.1.19'),
    (20, 20, 'Event registration', '2024-08-17 08:20:00', '192.168.1.20');



#--------------------------------------------- All Queries ---------------------------------------------#

-- Retrieve the oldest instructor/instructors
SELECT InstructorID, CONCAT(FIRSTNAME,LASTNAME) AS InstructorName,DateOfBirth
FROM instructors_t 
ORDER by DateOfBirth LIMIT 1;


-- Retrieve the names and descriptions of certifications along with the number of students who have applied for each certification.
SELECT C.CertificationName, C.CertificationDescription, COUNT(CA.StudentID) AS NumberOfApplicants
FROM Certifications_T C
LEFT JOIN CertificationApplications_T CA ON C.CertificationID = CA.CertificationID
GROUP BY C.CertificationName, C.CertificationDescription;


-- Find the total number of students who have enrolled in courses and the total number of students who have applied for certifications.
SELECT (SELECT COUNT(DISTINCT StudentID) FROM Enrollments_T) AS TotalCourseEnrollments,
	(SELECT COUNT(DISTINCT StudentID) FROM CertificationApplications_T) AS TotalCertificationApplications;
    

-- Retrieve the certifications that have more than 5 applications.
SELECT CertificationName
FROM Certifications_T
WHERE CertificationID IN (
    SELECT CertificationID
    FROM CertificationApplications_T
    GROUP BY CertificationID
    HAVING COUNT(ApplicationID) > 5
);


-- Find the event names, their corresponding dates, and the number of students registered for each event.
SELECT E.EventName, E.EventDate, COUNT(ER.StudentID) AS NumberOfRegistrations
FROM Events_T E
LEFT JOIN EventRegistrations_T ER ON E.EventID = ER.EventID
GROUP BY E.EventName, E.EventDate;


-- Retrieve the total amount of payments made by each student.
SELECT S.FirstName, S.LastName, SUM(P.Amount) AS TotalPayments
FROM Students_T S
LEFT JOIN Payments_T P ON S.StudentID = P.StudentID
GROUP BY S.FirstName, S.LastName;


-- List the certifications and the students who have applied for them, along with their application status.
SELECT C.CertificationName, S.FirstName, S.LastName, CA.ApplicationStatus
FROM Certifications_T C
JOIN CertificationApplications_T CA ON C.CertificationID = CA.CertificationID
JOIN Students_T S ON CA.StudentID = S.StudentID;


-- List the courses and their descriptions that are prerequisites for certifications.
SELECT C.CourseName, C.CourseDescription, CERT.CertificationName
FROM Courses_T C
JOIN CertificationRequirements_T CR ON C.CourseID = CR.CourseID
JOIN Certifications_T CERT ON CR.CertificationID = CERT.CertificationID;


-- Retrieve the average grade for each course.
SELECT C.CourseName, AVG(T.Grade) AS AverageGrade
FROM Courses_T C
LEFT JOIN Transcript_T T ON C.CourseID = T.CourseID
GROUP BY C.CourseName;


-- Find the users who have performed actions related to certifications.
SELECT U.Username, A.ActionTaken
FROM Users_T U
JOIN AuditLogs_T A ON U.UserID = A.UserID
WHERE A.ActionTaken LIKE '%Certification%';


-- Find the certifications that have all their requirements met (all courses have been completed).
SELECT CertificationName
FROM Certifications_T
WHERE CertificationID NOT IN (
    SELECT CR.CertificationID
    FROM CertificationRequirements_T CR
    LEFT JOIN Transcript_T T ON CR.CourseID = T.CourseID
    GROUP BY CR.CertificationID
    HAVING COUNT(CR.CourseID) <> COUNT(T.TranscriptID)
);


-- Retrieve the instructors who are associated with a department and have a specific role.
SELECT I.FirstName, I.LastName, FD.Roles, D.DepartmentName
FROM Instructors_T I
JOIN FacultyDepartments_T FD ON I.InstructorID = FD.InstructorID
JOIN Departments_T D ON FD.DepartmentID = D.DepartmentID
WHERE FD.Roles = 'Professor';


-- Retrieve names of the students who made payments through paypal
SELECT CONCAT(S.FirstName, S.LastName) AS StudentName, P.PaymentMethod 
FROM Students_T S
LEFT JOIN Payments_T P ON S.StudentID = P.StudentID
WHERE P.PaymentMethod='PayPal'
GROUP BY S.FirstName, S.LastName;


-- Retrieve the permissions and its description that each role has 
SELECT r.RoleId,r.RoleName,p.PermissionName,p.PermissionDescription
FROM rolepermissions_t rp 
JOIN roles_t r ON rp.RoleID = r.RoleID
JOIN permissions_t p ON rp.PermissionID = p.PermissionID;


-- Retrieve the students who received an A in a specific course in a particular Semester
SELECT s.StudentID,concat(s.FirstName,s.Lastname) AS StudentName,t.TranscriptID,t.CourseID,c.CourseName,t.Grade,t.Semester
FROM transcript_t t 
JOIN students_t s ON t.StudentID = s.StudentID
JOIN courses_t c ON t.CourseID = c.CourseID
WHERE t.Grade='A' and t.Semester = 'SPRING 2023' and c.CourseID = '4';




#--------------------------------------------- Stored Procedures ---------------------------------------------#

-- Stored Procedure to Calculate Student's Total Credits
DELIMITER //
CREATE PROCEDURE CalculateTotalCredits(IN studentID INT, OUT totalCredits INT)
BEGIN
    SET totalCredits = (
        SELECT SUM(Credits)
        FROM Enrollments_T E
        JOIN Courses_T C ON E.CourseID = C.CourseID
        WHERE E.StudentID = studentID
    );
END;
//
DELIMITER ;

-- to call the procedure
CALL CalculateTotalCredits(1, @totalCredits);
SELECT @totalCredits;



-- Stored Procedure to Approve Certification Application
DELIMITER //
CREATE PROCEDURE ApproveCertificationApplication(IN appID INT)
BEGIN
    UPDATE CertificationApplications_T
    SET ApplicationStatus = 'Approved'
    WHERE ApplicationID = appID;
END;
//
DELIMITER ;

-- to call the procedure
CALL ApproveCertificationApplication(1);



-- Stored Procedure to Get Student's Transcript
DELIMITER //
CREATE PROCEDURE GetStudentTranscript(IN studentID INT)
BEGIN
    SELECT C.CourseName, T.Grade
    FROM Courses_T C
    JOIN Transcript_T T ON C.CourseID = T.CourseID
    WHERE T.StudentID = studentID;
END;
//
DELIMITER ;

-- to call the procedure
CALL GetStudentTranscript(1);

-- Stored procedure to authenticate a certificate
DROP PROCEDURE AuthenticateCertificate_p;
DELIMITER //
CREATE PROCEDURE AuthenticateCertificate_p(
IN certificateID_param INT,
IN studentID_param INT,
OUT message VARCHAR(50)
)
BEGIN
DECLARE certificate_id_var INT;
DECLARE studentID_var INT;
DECLARE approval_date_var DATE;
DECLARE approval_year_var YEAR;
SELECT CertificationID, StudentID, ApprovalDate, 
YEAR(ApprovalDate)
INTO certificate_id_var, studentID_var, approval_date_var,
approval_year_var
FROM certificationapplications_t INNER JOIN 
certificationapprovals_t
ON certificationapplications_t.ApplicationID=
certificationapprovals_t.ApplicationID
WHERE certificationapprovals_t.ApprovalStatus='Approved'
AND CertificationID=certificateID_param and 
StudentID=studentID_param; 

IF approval_year_var>'1900' THEN
	SET message='Student received the certificate.';
ELSE 
	SET message='Student did not receive the certificate';
END IF;
END//

CALL AuthenticateCertificate_p(
1,10,
@message
);

SELECT @message;

select * from certificationapprovals_t;
SELECT CertificationID, StudentID, ApprovalDate
FROM certificationapplications_t INNER JOIN 
certificationapprovals_t
ON certificationapplications_t.ApplicationID=
certificationapprovals_t.ApplicationID
WHERE certificationapprovals_t.ApprovalStatus='Approved'
AND CertificationID=1 and 
StudentID=1;

-- Stored procedure for count of students falling in each grade

use buan6320_008_groups9;
select * from buan6320_008_groups9.Transcript_T;
DELIMITER //
CREATE PROCEDURE GetStudentGradeCount()
BEGIN
    SELECT grade, COUNT(*) AS grade_count
    FROM Transcript_T
    GROUP BY grade
    ORDER BY grade_count DESC;
END //
DELIMITER ;
CALL GetStudentGradeCount();






-- Stored procedure that accepts StudentID as parameter and returns all details specific to that parameter 
-- about Event Registrations

use buan6320_008_groups9;
DELIMITER //
CREATE PROCEDURE GetStudentDetails(IN sID INT)
BEGIN
  SELECT * FROM EventRegistrations_t WHERE StudentID = sID;
END;
//
DELIMITER ;
CALL GetStudentDetails(1);
#--------------------------------------------- Functions ---------------------------------------------#


-- Function to Calculate Student's Total Credits
DELIMITER //
CREATE FUNCTION CalculateTotalCredits(studentID INT) RETURNS INT
deterministic
BEGIN
    DECLARE totalCredits INT;
    SET totalCredits = (
        SELECT SUM(Credits)
        FROM Enrollments_T E
        JOIN Courses_T C ON E.CourseID = C.CourseID
        WHERE E.StudentID = studentID
    );
    RETURN totalCredits;
END;
//
DELIMITER ;

-- function call
SELECT CalculateTotalCredits(1);



-- Function to Calculate Average Grade for a Course
DELIMITER //
CREATE FUNCTION CalculateAverageGrade(courseID INT) RETURNS DECIMAL(4, 2) deterministic
BEGIN
    DECLARE avgGrade DECIMAL(4, 2);
    SELECT AVG(Grade) INTO avgGrade
    FROM Transcript_T
    WHERE CourseID = courseID;
    RETURN avgGrade;
END;
//
DELIMITER ;

-- function call
SELECT CalculateAverageGrade(101);


-- Function to Check If a Student Meets Certification Prerequisites
DELIMITER //
CREATE FUNCTION CheckPrerequisites(studentID INT, certificationID INT) RETURNS BOOLEAN deterministic
BEGIN
    DECLARE meetsPrerequisites BOOLEAN;
    SET meetsPrerequisites = NOT EXISTS (
        SELECT 1
        FROM CertificationRequirements_T CR
        WHERE CR.CertificationID = certificationID
        AND CR.CourseID NOT IN (
            SELECT CourseID
            FROM Transcript_T
            WHERE StudentID = studentID
        )
    );
    RETURN meetsPrerequisites;
END;
//
DELIMITER ;

-- function call
SELECT CheckPrerequisites(1, 1001); 




#--------------------------------------------- Triggers ---------------------------------------------#


-- Trigger to Calculate Average Grade After Each Grade Insertion
DELIMITER //
CREATE TRIGGER UpdateAverageGrade
AFTER INSERT ON Transcript_T
FOR EACH ROW
BEGIN
    UPDATE Courses_T
    SET AverageGrade = (
        SELECT AVG(Grade)
        FROM Transcript_T
        WHERE CourseID = NEW.CourseID
    )
    WHERE CourseID = NEW.CourseID;
END;
//
DELIMITER ;



-- Trigger to Prevent Certification Application if Prerequisites Are Not Met
DELIMITER //
CREATE TRIGGER PreventCertificationApplication
BEFORE INSERT ON CertificationApplications_T
FOR EACH ROW
BEGIN
    IF NOT EXISTS (
        SELECT 1
        FROM CertificationRequirements_T CR
        WHERE CR.CertificationID = NEW.CertificationID
        AND CR.CourseID NOT IN (
            SELECT CourseID
            FROM Transcript_T
            WHERE StudentID = NEW.StudentID
        )
    ) THEN
        SIGNAL SQLSTATE '45000'
        SET MESSAGE_TEXT = 'Student does not meet certification prerequisites';
    END IF;
END;
//
DELIMITER ;



-- Trigger to Update Student Certification Status After Application Approval
DELIMITER //
CREATE TRIGGER UpdateCertificationStatus
AFTER UPDATE ON CertificationApplications_T
FOR EACH ROW
BEGIN
    IF NEW.ApplicationStatus = 'Approved' THEN
        UPDATE Students_T
        SET CertificationStatus = 'Certified'
        WHERE StudentID = NEW.StudentID;
    END IF;
END;
//
DELIMITER ;


-- After insert trigger to check PaymentMethod and display a message 

create table message(
Message varchar(30));

use buan6320_008_groups9;

DELIMITER //
create trigger check_PaymentMethod
after insert
on Payments_T
for each row
begin
if new.PaymentMethod = 'Credit Card' then
insert into message( Message)
values('20% cashback');
end if;
end //
delimiter ;


-- INSERT INTO Payments_T (PaymentID, StudentID, Amount, PaymentDate, PaymentMethod)
--	VALUES(21, 1, 200.00, '2023-01-15', 'Credit Card');
select*from message;
