# Databse-Project_1


-- TODO: create the courses database
CREATE DATABASE courses;
-- TODO: "open" the database for use
\c courses;

DROP TABLE IF EXISTS instructors;
DROP TABLE IF EXISTS Courses;
DROP TABLE IF EXISTS "Sections" ;

-- TODO: create table instructors
CREATE TABLE instructors (
	  email      VARCHAR(100) NOT Null,
	 "name"      VARCHAR(255),
	  title      VARCHAR(100) NULL,
	  office     VARCHAR(10) NULL,
	  hours      VARCHAR(20) NULL,
	  PRIMARY KEY (email)
);


-- TODO: create table courses
CREATE TABLE Courses(
	prefix      VARCHAR(3) NOT NULL ,
    "number"    VARCHAR(4) NOT NULL ,
	title       varchar(200),
	description TEXT ,
	credit      INTEGER,
	prereqs     TExt NULL,
	UNIQUE (prefix, "number"),
	PRIMARY KEY (prefix, "number")
);


-- TODO: create table sections
CREATE TABLE "Sections"(
	  crn        INTEGER NOT NULL,
	  prefix     VARCHAR(3) NoT NULL ,
	 "number"    VARCHAR(4) NOT NULL ,
	 "section"   VARCHAR(4),
	  semsiter   VARCHAR(10),
	 "year"      INTEGER,
	  instructor VARCHAR(100) NULL,
	  times      VARCHAR(100) NULL,
	 "start"     DATE   NULL,
	 "end"       DATE NULL,
	 "location"  VARCHAR(250) NULL,
      campus     VARCHAR(250) NULL,
	 FOREIGN KEY (prefix,"number") REFERENCES Courses(prefix, "number"),
	 PRIMARY KEY(crn),
	 FOREIGN KEY (instructor) REFERENCES instructors(email)
	
);

-- TODO: manually insert a few instructors
INSERT INTO instructors VALUES
	('codd@msudenver.edu','Coddy, Egar', ' father of the relational model'),
	('cohenb@msudenver.edu','Cohen,Blanche','Adjunct Instructor'),
	('rranjidh@msudenver.edu','Rajan,Ranjidha','Adjunct Instructor'),
	('fjiang@msudenver.edu','Jiang,Feng','Adjunct Instructor'),
	( 'aibrahi8@msudenver.edu','Ibrahim,Adil','Adjunct Instructor'),
	( 'tle61@msudenver.edu','Le,ThienNgo', 'Adjunct Instructor');


-- TODO: manually insert a few courses
INSERT INTO courses VALUES
('CS',1030, 'Computer Science Principles','Computer Science Principles introduces students to the central ideas of computer science vital for success in today’s world',4,'no prereqs'),
('CS',1050,'Computer Science 1','This is the first course in the computer science core sequence',4, 'CS 1030'),
('CS',1400, 'Computer Organization 1','In this course, students will study the internal organization, characteristics, performance and interactions of a computer system',4, 'An intermediate algebra course'),
('CS',2050,'Computer Science 2', ' This course, a continuation of CS 1050, further emphasizes the concepts of the software development cycle and introduces the concept of an abstract data type (ADT)',4,'CS 1050 and MTH 111');


INSERT INTO "Sections" VALUES

('30662','CS',1050, '005', 'Spring','2022','cohenb@msudenver.edu','TR 10:00 am - 11:50 am', '2022-01-18','2022-05-14', 'R285','Aerospace and Engineering Sci'),
('31998','CS',1030, '001','Spring', '2022','cohenb@msudenver.edu', 'TR 12:00 PM -01:50 PM','2022-01-18', ' 2022-05-14','R285', 'Aerospace and Engineering Sci' ),
('31999','CS', 1030, '002', 'Spring','2022','cohenb@msudenver.edu','MW 12:00 PM - 01:50 PM', '2022-01-18','2022-05-14', 'R285','Aerospace and Engineering Sci'),
('32000','CS', 1030, '003', 'Spring','2022','cohenb@msudenver.edu','MW 04:00 PM - 05:50 PM', '2022-01-18', '2022-05-14', 'R285','Aerospace and Engineering Sci'),
('30640','CS',1050, '001', 'Spring','2022','aibrahi8@msudenver.edu','MW 10:00 Am - 11:50 AM', '2022-01-18', '2022-05-14', 'R285','Aerospace and Engineering Sci'),
('30641','CS',1050, '002', 'Spring','2022','rranjidh@msudenver.edu','MW 12:00 PM - 01:50 PM', '2022-01-18','2022-05-14', 'R220','Aerospace and Engineering Sci'),
('31296','CS',1400, '001', 'Spring','2022','fjiang@msudenver.edu','TR 12:00 PM - 01:50 PM', '2022-01-18', '2022-05-14', 'R210','Aerospace and Engineering Sci'),
('34663','CS',1400, '003', 'Spring','2022','rranjidh@msudenver.edu','MW 04:00 PM - 05:50 PM', '2022-01-18','2022-05-14', 'R210','Aerospace and Engineering Sci'),
('30644','CS',2050, '002', 'Spring','2022','tle61@msudenver.edu','MW 06:00 PM - 07:50 PM', '2022-01-18','2022-05-14', 'R210','Aerospace and Engineering Sci'),
('30643','CS',2050, '001', 'Spring','2022','aibrahi8@msudenver.edu','TR 02:00 PM - 03:50 PM', '2022-01-18','2022-05-14', 'R210','Aerospace and Engineering Sci');



-- TODO: the total number of courses (name the count as "total")
SELECT (COUNT(*)) AS Total FROM Courses;
-- TODO: a list of all courses prefix, number, and title, sorted by prefix and then number
SELECT * FROM courses
         ORDER BY prefix, "number";
		 
-- TODO: an alphabetical list of all instructors in the database
SELECT * FROM instructors
         ORDER BY "name";
		 
-- TODO: the prefix, number, section, and (course) title of all courses sections in the database, sorted by prefix, number and section
SELECT c.prefix, c."number",s."section", title FROM courses c
		 JOIN "Sections" S ON c.prefix = S.prefix AND c."number" = s."number" 
		 ORDER BY prefix, "number","section";
		 
-- TODO: the prefix, number, the number of sections (named as "sections"), and (course) title of all courses sections in the database, sorted by prefix and number
SELECT c.prefix, c."number", COUNT(s."section") AS "sections" FROM courses c
         JOIN "Sections" s ON  c.prefix = s.prefix AND c."number"= s."number"
		 GROUP BY c."number", c.prefix
		 ORDER BY C.prefix, c."number";
		 
-- TODO: an alphabetical list of the instructors that are teaching CS 1050 or CS 2050 (must avoid showing names repeated)
SELECT   DISTINCT "name", email FROM instructors i
         JOIN "Sections" s ON i.email = s.instructor
		 WHERE "number" ='2050' OR "number" ='1050'
		 ORDER BY "name" ASC;	
		 
-- TODO: show an alphabetical list of instructors followed by the number of sections (named as "sections") that they are teaching, sorted in descending order of "sections"
SELECT "name" ,COUNT (s."section" )AS sections From instructors i
        JOIN "Sections" s ON i.email = s.instructor
	    GROUP BY "name"
	    ORDER BY sections DESC;
-- TODO: same as before, but limit the output to the top 3 instructors based on the number of sections that they are teaching
  SELECT "name", COUNT (s."section" )AS sections From instructors i
        JOIN "Sections" s ON i.email = s.instructor
	    GROUP BY "name"
	    ORDER BY sections DESC
		LIMIT 3;
	
	 
-- TODO: show an alphabetical list of the instructor(s) that are NOT currently teaching a section this semester 
SELECT i."name", i.email FROM instructors i
      LEFT JOIN "Sections" s on i.email = s.instructor
	   WHERE  s.instructor IS NULL;
-- TODO: show the sections (with the instructor assigned to them) of CS 1050 that are being offered in the spring (2022) on TR 10:00am-11:50am
SELECT i."name", s.prefix, s.number FROM instructors i
             JOIN "Sections" s  ON i.email = s.instructor
			 WHERE s."number" = '1050' AND s."year" = '2022' AND s.times ='TR 10:00 am - 11:50 am';
			 
