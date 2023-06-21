## 1.

#### (a)
```sql
UPDATE course 
SET course.CreditHour = 3 
WHERE course.CourseName = "DatabaseSystem" AND course.Department = "EECS"
```
#### (b)
```sql
DELETE FROM student
WHERE student.`Name` = "Edward" AND student.StudentNumber = "001"
```
#### (c)
```sql
SELECT 
	course.CourseName
FROM course
JOIN section ON course.CourseNumber = section.CourseNumber
WHERE
	section.Instructor = "Liu" AND 
	section.`Year` BETWEEN 2020 AND 2022
```
#### (d)
```sql
SELECT 
	section.CourseNumber, section.Semester, section.`Year`,COUNT(*)
FROM
	section, grade_report
WHERE
	section.Instructor = "Liu" AND 
	grade_report.SectionNumber = section.SectionNumber
GROUP BY
	grade_report.SectionNumber
```
#### (e)
```sql
SELECT precourse.CourseName,precourse.CourseNumber
FROM course
JOIN prerequisite ON course.CourseNumber = prerequisite.CourseNumber
JOIN course AS precourse ON prerequisite.PrerequisiteCourseNumber = precourse.CourseNumber
WHERE course.CourseName = "Web Programing" AND course.Department = "EECS"								
```
#### (f)
```sql
SELECT student.`Name`, course.CourseNumber, course.CourseName, course.CreditHour, section.Semester, section.`Year`,grade_report.Grade
FROM student
JOIN grade_report ON student.StudentNumber = grade_report.StudentNumber
JOIN section ON grade_report.SectionNumber = section.SectionNumber
JOIN course ON section.CourseNumber = course.CourseNumber
WHERE student.Class = '3' AND
	  student.Major = "EECS" 
```
#### (g)
```sql
SELECT student.`Name`
FROM student
JOIN grade_report ON student.StudentNumber = grade_report.StudentNumber
GROUP BY student.`Name` HAVING MIN(grade_report.Grade) >= 80
```
#### (h)
```sql
SELECT student.`Name`,student.Major
FROM student
JOIN grade_report ON student.StudentNumber = grade_report.StudentNumber
GROUP BY student.`Name` HAVING MIN(grade_report.Grade) >= 60
```
#### (i)
```sql
SELECT student.`Name`,student.Major
FROM student
JOIN grade_report ON student.StudentNumber = grade_report.StudentNumber
GROUP BY student.`Name` HAVING MAX(grade_report.Grade) < 60
ORDER BY student.`Name` ASC
```
#### (j)
```sql
SELECT student.`Name`,student.Major,AVG(grade_report.Grade)
FROM student
JOIN grade_report ON student.StudentNumber = grade_report.StudentNumber
JOIN section ON grade_report.SectionNumber = section.SectionNumber
WHERE section.`Year` = '2022'
GROUP BY student.`Name` HAVING AVG(grade_report.Grade) >= 80.0
```
#### (k)
```sql
SELECT MA.Major,COUNT(Major)
FROM(
SELECT student.StudentNumber,student.Major
FROM student
JOIN grade_report ON student.StudentNumber = grade_report.StudentNumber
GROUP BY student.`Name`,student.Major HAVING AVG(grade_report.Grade) < 60) as MA
GROUP BY MA.Major
```
#### (l)
```sql
CREATE VIEW student_data
AS SELECT student.StudentNumber, student.`Name`, course.CourseName, section.Semester, section.`Year`, grade_report.Grade
FROM student
JOIN grade_report ON student.StudentNumber = grade_report.StudentNumber
JOIN section ON grade_report.SectionNumber = section.SectionNumber
JOIN course ON section.CourseNumber = course.CourseNumber
```
## 2.
#### (a)
```sql
SET FOREIGN_KEY_CHECKS=0;

CREATE TABLE `helloworld`.`COURSE`  (
  `CourseNumber` int NOT NULL,
  `CourseName` varchar(255) NULL,
  `CreditHour` varchar(255) NULL,
  `Department` varchar(255) NULL,
  PRIMARY KEY (`CourseNumber`)
);

CREATE TABLE `helloworld`.`GRADE_REPORT`  (
  `StudentNumber` int NOT NULL,
  `SectionNumber` int NOT NULL,
  `Grade` varchar(255) NULL,
  PRIMARY KEY (`StudentNumber`, `SectionNumber`),
  CONSTRAINT `fk_GRADE_REPORT_STUDENT_1` FOREIGN KEY (`StudentNumber`) REFERENCES `helloworld`.`STUDENT` (`StudentNumber`),
  CONSTRAINT `fk_GRADE_REPORT_SECTION_1` FOREIGN KEY (`SectionNumber`) REFERENCES `helloworld`.`SECTION` (`SectionNumber`)
);

CREATE TABLE `helloworld`.`PREREQUISITE`  (
  `CourseNumber` int NOT NULL,
  `PrerequisiteCourseNumber` int NOT NULL,
  PRIMARY KEY (`CourseNumber`, `PrerequisiteCourseNumber`),
  CONSTRAINT `fk_PREREQUISITE_COURSE_1` FOREIGN KEY (`CourseNumber`) REFERENCES `helloworld`.`COURSE` (`CourseNumber`)
);

CREATE TABLE `helloworld`.`SECTION`  (
  `SectionNumber` int NOT NULL,
  `CourseNumber` int NULL,
  `Semester` varchar(255) NULL,
  `Year` varchar(255) NULL,
  `Instructor` varchar(255) NULL,
  PRIMARY KEY (`SectionNumber`)
);

CREATE TABLE `helloworld`.`STUDENT`  (
  `StudentNumber` varchar(255) NOT NULL,
  `Name` varchar(255) NULL,
  `Class` varchar(255) NULL,
  `Major` varchar(255) NULL,
  PRIMARY KEY (`StudentNumber`)
);

SET FOREIGN_KEY_CHECKS=1;
```
#### (b)
```sql
INSERT INTO `course` VALUES (159, 'DatabaseSystem', '3', 'EECS');
INSERT INTO `course` VALUES (369, 'AI', '3', 'EECS');
INSERT INTO `course` VALUES (753, 'Web Programing', '3', 'EECS');
INSERT INTO `course` VALUES (842, 'Network Computer', '3', 'CSIE');
INSERT INTO `grade_report` VALUES (11038025, 1111, '59');
INSERT INTO `grade_report` VALUES (109590009, 1111, '85');
INSERT INTO `grade_report` VALUES (109590009, 1112, '90');
INSERT INTO `grade_report` VALUES (109590009, 1113, '77');
INSERT INTO `grade_report` VALUES (109590009, 1114, '70');
INSERT INTO `grade_report` VALUES (109590032, 1111, '110');
INSERT INTO `grade_report` VALUES (109590032, 1112, '99');
INSERT INTO `grade_report` VALUES (109590032, 1113, '88');
INSERT INTO `grade_report` VALUES (109590032, 1114, '95');
INSERT INTO `grade_report` VALUES (120000013, 1111, '53');
INSERT INTO `grade_report` VALUES (120000013, 1112, '51');
INSERT INTO `prerequisite` VALUES (753, 159);
INSERT INTO `prerequisite` VALUES (159, 369);
INSERT INTO `prerequisite` VALUES (369, 753);
INSERT INTO `section` VALUES (1111, 159, '110S', '2022', 'Liu');
INSERT INTO `section` VALUES (1112, 753, '110W', '2023', 'Wu');
INSERT INTO `section` VALUES (1113, 369, '109S', '2021', 'Ling');
INSERT INTO `section` VALUES (1114, 842, '109W', '2020', 'Liu');
INSERT INTO `student` VALUES (11038025, 'Sophia', '3', 'ID');
INSERT INTO `student` VALUES (109590009, 'Jason', '3', 'EECS');
INSERT INTO `student` VALUES (109590032, 'Roy', '3', 'EECS');
INSERT INTO `student` VALUES (120000013, 'Emily', '3', 'EECS');
```
#### (c)
##### 1-a
![[Pasted image 20221211202000.png]]
![[Pasted image 20221211202025.png]]
##### 1-b
![[Pasted image 20221211202301.png]]
![[Pasted image 20221211202237.png]]
![[Pasted image 20221211202323.png]]
##### 1-c
![[Pasted image 20221211205955.png]]
##### 1-d
![[Pasted image 20221211210149.png]]
##### 1-e
![[Pasted image 20221211203139.png]]
##### 1-f
![[Pasted image 20221211204052.png]]
##### 1-g
![[Pasted image 20221211204126.png]]
##### 1-h
![[Pasted image 20221211204152.png]]
##### 1-i
![[Pasted image 20221211210449.png]]
##### 1-j
![[Pasted image 20221211204308.png]]
##### 1-k
![[Pasted image 20221211211133.png]]
##### 1-l
![[Pasted image 20221211204510.png]]
![[Pasted image 20221211211250.png]]
#### (d)
```sql
CREATE PROCEDURE student_data (
    IN student_id VARCHAR(255),
    OUT result_str VARCHAR(255)
)
BEGIN
    SET @average_grade := 0;
    SELECT AVG(grade_report.Grade)
    INTO @average_grade
    FROM student
    JOIN grade_report ON student.StudentNumber = grade_report.StudentNumber
    WHERE student.StudentNumber = student_id;
    
    
    IF @average_grade >= 60 THEN
        SET result_str = 'PASS';
    ELSE
        SET result_str = 'FAIL';
    END IF;

END
```
![[Pasted image 20221211214322.png]]
![[Pasted image 20221211215114.png]]
