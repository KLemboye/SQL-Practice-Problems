# Problem 1  - ["Essential SQL" by Kris Wenzel](https://www.essentialsql.com/contains-in-sql/)
This practice problem contains 5 questions focused around a school environment.

![alt text](https://github.com/KLemboye/SQL-Practice-Problems/blob/49742bdd75c2297a24e9c0e5e541cb2f59db23e7/School%20Data%20Model.JPG "School Data Model")

**1. The schools would like to know which student has the highest GPA. To qualify students, need to have taken two or more courses.
List the Student Name, number of courses taken, and overall GPA.**
```
SELECT TOP 1 P.FirstName + ' ' + P.LastName AS StudentName, COUNT(SG.EnrollmentID) AS NumberOfCourseTaken, ROUND(SUM(Grade*Credits)/SUM(C.Credits), 1) AS GPA
FROM StudentGrade AS SG
INNER JOIN Person AS P 
ON SG.StudentID = P.PersonID
INNER JOIN Course AS C
ON SG.CourseID = C.CourseID
GROUP BY P.FirstName + ' ' + P.LastName
HAVING COUNT(SG.EnrollmentID) >= 2
ORDER BY GPA DESC
```
**2. In order to plan for some new hires, the school would like to know which course is most popular.
Create a cross tab counting the number of courses taken. The row should be student enrollment year, the columns, course title.**
```
SELECT EnrollmentYear, Calculus, Chemistry, Physics, Composition, Poetry, Literature, Trigonometry, Microeconomics, Macroeconomics, Quantitative
FROM
	(
	SELECT DISTINCT YEAR(P.EnrollmentDate) AS EnrollmentYear, C.Title AS CourseTitle, SG.EnrollmentID AS EnrollmentID
	FROM Course AS C
	INNER JOIN StudentGrade AS SG
	ON C.CourseID = SG.CourseID
	INNER JOIN Person AS P
	ON P.PersonID = SG.StudentID
	)
AS PivotData
PIVOT(
	COUNT(EnrollmentID)
	FOR CourseTitle
	IN(Calculus, Chemistry, Physics, Composition, Poetry, Literature, Trigonometry, Microeconomics, Macroeconomics, Quantitative)
	) AS PivotResult
ORDER BY EnrollmentYear DESC
```
**3. The facilities manager would like to create a directory of Instructor Offices. To help them out, you’re going to provide them a result containing the Building Name, Office Number, and Instructor Name.
The results should be ordered by Building Name and then Office 
Number.**
```
SELECT SUBSTRING(OA.Location, (CHARINDEX(' ', OA.Location)) + 1, (LEN(OA.Location)) - (CHARINDEX(' ' ,OA.Location))) AS 'Building Name',
	SUBSTRING(OA.Location, 1, (CHARINDEX(' ', OA.Location))) AS 'Office Number',
	CONCAT(P.FirstName, ' ', P.LastName) AS 'Instructor Name'
FROM OfficeAssignment AS OA
INNER JOIN Person AS P
ON OA.InstructorID = P.PersonID
ORDER BY 'Building Name', 'Office Number'
```
**4. There is a fear the Person table, which isn’t normalized, has a data issue. The column discriminator should either be “Instructor” or “Student” depending on whether HireDate or EnrollmentDate, respectively are filled.
Write a query to check the discriminator colum is correctly based on HireDate and EnrollmentDate. Your query should output the PersonID, LastName, DatePresent (Hire or Enrollment), Flag. The Flag will be “Discriminator OK” if the data check out, and “Discriminator Bad” if it doesn’t.**
```
SELECT PersonID,
	LastName, 
	CASE
		WHEN HireDate IS NULL THEN EnrollmentDate
		WHEN EnrollmentDate IS NULL THEN HireDate
	END AS DatePresent,
	CASE
		WHEN HireDate IS NULL AND EnrollmentDate IS NOT NULL AND Discriminator = 'Student' THEN 'Discriminator OK'
		WHEN EnrollmentDate IS NULL AND HireDate IS NOT NULL AND Discriminator = 'Instructor' THEN 'Discriminator OK'
		ELSE 'Discriminator Bad'
	END AS Flag
FROM Person
```
**5. Create course schedules for all student’s onsite courses Include the Student Name, Department Name, Course Title, Location, Days and Time.
Order the schedule by student last name, department name, and course id.**
```
SELECT P.FirstName + ' ' + P.LastName AS 'Student Name',
	D.Name AS 'Department Name',
	c.Title AS 'Course Title',
	OC.Location AS 'Location',
	OC.Days,
	OC.Time
FROM Course AS C
INNER JOIN OnsiteCourse AS OC
ON C.CourseID = OC.CourseID
INNER JOIN StudentGrade AS SG
ON C.CourseID = SG.CourseID
INNER JOIN Person AS P
ON SG.StudentID = P.PersonID
INNER JOIN Department AS D
ON C.DepartmentID = D.DepartmentID
ORDER BY P.LastName,
	D.Name,
	C.CourseID
```
