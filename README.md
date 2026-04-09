# MY-SQL-LEARN
-- 1. Naya Database banate hain--
CREATE DATABASE Intellipaat_CS2;
GO

-- 2. database ko use karne ki command--
USE Intellipaat_CS2;
GO

-- 3. LOCATION Table create aur data insert
CREATE TABLE LOCATION (
    Location_ID INT PRIMARY KEY,
    City VARCHAR(50)
);

INSERT INTO LOCATION (Location_ID, City) VALUES 
(122, 'New York'),
(123, 'Dallas'),
(124, 'Chicago'),
(167, 'Boston');

-- 4. DEPARTMENT Table create aur data insert
CREATE TABLE DEPARTMENT (
    Department_Id INT PRIMARY KEY,
    Name VARCHAR(50),
    Location_Id INT FOREIGN KEY REFERENCES LOCATION(Location_ID)
);

INSERT INTO DEPARTMENT (Department_Id, Name, Location_Id) VALUES 
(10, 'Accounting', 122),
(20, 'Sales', 124),
(30, 'Research', 123),
(40, 'Operations', 167);

-- 5. JOB Table create aur data insert
CREATE TABLE JOB (
    Job_ID INT PRIMARY KEY,
    Designation VARCHAR(50)
);

INSERT INTO JOB (Job_ID, Designation) VALUES 
(667, 'Clerk'),
(668, 'Staff'),
(669, 'Analyst'),
(670, 'Sales Person'),
(671, 'Manager'),
(672, 'President');

-- 6. EMPLOYEE Table create aur data insert
CREATE TABLE EMPLOYEE (
    Employee_Id INT PRIMARY KEY,
    Last_Name VARCHAR(50),
    First_Name VARCHAR(50),
    Middle_Name CHAR(1),
    Job_Id INT FOREIGN KEY REFERENCES JOB(Job_ID),
    Hire_Date DATE,
    Salary DECIMAL(10,2),
    Comm DECIMAL(10,2),
    Department_Id INT FOREIGN KEY REFERENCES DEPARTMENT(Department_Id)
);

INSERT INTO EMPLOYEE (Employee_Id, Last_Name, First_Name, Middle_Name, Job_Id, Hire_Date, Salary, Comm, Department_Id) VALUES 
(7369, 'Smith', 'John', 'Q', 667, '1984-12-17', 800, NULL, 20),
(7499, 'Allen', 'Kevin', 'J', 670, '1985-02-20', 1600, 300, 30),
(755, 'Doyle', 'Jean', 'K', 671, '1985-04-04', 2850, NULL, 30),
(756, 'Dennis', 'Lynn', 'S', 671, '1985-05-15', 2750, NULL, 30),
(757, 'Baker', 'Leslie', 'D', 671, '1985-06-10', 2200, NULL, 40),
(7521, 'Wark', 'Cynthia', 'D', 670, '1985-02-22', 1250, NULL, 30);
USE Intellipaat_CS2;
SELECT * FROM EMPLOYEE;
SELECT * FROM DEPARTMENT;
SELECT * FROM JOB;
SELECT * FROM LOCATION;
---List out the First Name, Last Name, Salary, Commission for all Employees.--
SELECT First_Name, Last_Name, Salary, Comm FROM EMPLOYEE;

----List out the Employee ID, Last Name, Department ID for all employees and
--alias---
SELECT 
    Employee_Id AS "ID of the Employee", 
    Last_Name AS "Name of the Employee", 
    Department_Id AS "Dep_id" 
FROM EMPLOYEE;

----List out the annual salary of the employees with their names only.
SELECT 
    First_Name, 
    Last_Name, 
    (Salary * 12) AS "Annual Salary" 
FROM EMPLOYEE;

List the details about "Smith"
SELECT * FROM EMPLOYEE 
WHERE Last_Name = 'Smith';

---List out the employees who are working in department 20.
SELECT * FROM EMPLOYEE 
WHERE Department_Id = 20;

--List out the employees who are earning salary between 2000 and 3000..
SELECT * FROM EMPLOYEE 
WHERE Salary BETWEEN 2000 AND 3000;

---List out the employees who are working in department 10 or 20.
SELECT * FROM EMPLOYEE 
WHERE Department_Id IN (10, 20);

---Find out the employees who are not working in department 10 or 30.
SELECT * FROM EMPLOYEE 
WHERE Department_Id NOT IN (10, 30);

---List out the employees whose name starts with 'L'.
SELECT * FROM EMPLOYEE 
WHERE First_Name LIKE 'L%';

---List out the employees whose name starts with 'L' and ends with 'E'.
SELECT * FROM EMPLOYEE 
WHERE First_Name LIKE 'L%e';

---List out the employees whose name length is 4 and start with 'J'.
SELECT * FROM EMPLOYEE 
WHERE First_Name LIKE 'J___';

---List out the employees who are working in department 30 and draw the
salaries more than 2500.
SELECT * FROM EMPLOYEE 
WHERE Department_Id = 30 AND Salary > 2500;

---List out the employees who are not receiving commission.
SELECT * FROM EMPLOYEE 
WHERE Comm IS NULL;

---List out the Employee ID and Last Name in ascending order based on the
---Employee ID.
SELECT Employee_Id, Last_Name 
FROM EMPLOYEE 
ORDER BY Employee_Id ASC;

---List out the Employee ID and Name in descending order based on salary.
SELECT Employee_Id, First_Name, Last_Name, Salary 
FROM EMPLOYEE 
ORDER BY Salary DESC;

---List out the employee details according to their Last Name in ascending-order.
SELECT * FROM EMPLOYEE 
ORDER BY Last_Name ASC;

---List out the employee details according to their Last Name in ascending
--order and then Department ID in descending order.---
SELECT * FROM EMPLOYEE 
ORDER BY Last_Name ASC, Department_Id DESC;

---List out the department wise maximum salary, minimum salary and
--average salary of the employees.----
SELECT 
    Department_Id, 
    MAX(Salary) AS Max_Salary, 
    MIN(Salary) AS Min_Salary, 
    AVG(Salary) AS Avg_Salary 
FROM EMPLOYEE 
GROUP BY Department_Id;

---List out the job wise maximum salary, minimum salary and average
--salary of the employees.---
SELECT 
    Job_Id, 
    MAX(Salary) AS Max_Salary, 
    MIN(Salary) AS Min_Salary, 
    AVG(Salary) AS Avg_Salary 
FROM EMPLOYEE 
GROUP BY Job_Id;

---List out the number of employees who joined each month in ascending order.---
SELECT 
    MONTH(Hire_Date) AS Join_Month, 
    COUNT(Employee_Id) AS Number_of_Employees 
FROM EMPLOYEE 
GROUP BY MONTH(Hire_Date) 
ORDER BY Join_Month ASC;

---List out the number of employees for each month and year in ascending order based on the year and month.---
SELECT 
    YEAR(Hire_Date) AS Join_Year, 
    MONTH(Hire_Date) AS Join_Month, 
    COUNT(Employee_Id) AS Number_of_Employees 
FROM EMPLOYEE 
GROUP BY YEAR(Hire_Date), MONTH(Hire_Date) 
ORDER BY Join_Year ASC, Join_Month ASC;

---List out the Department ID having at least four employees.
SELECT Department_Id 
FROM EMPLOYEE 
GROUP BY Department_Id 
HAVING COUNT(Employee_Id) >= 4;

---How many employees joined in February month.
SELECT COUNT(Employee_Id) AS Feb_Joinees 
FROM EMPLOYEE 
WHERE MONTH(Hire_Date) = 2;

---How many employees joined in May or June month.
SELECT COUNT(Employee_Id) AS May_June_Joinees 
FROM EMPLOYEE 
WHERE MONTH(Hire_Date) IN (5, 6);

---How many employees joined in 1985?
SELECT COUNT(Employee_Id) AS Joinees_1985 
FROM EMPLOYEE 
WHERE YEAR(Hire_Date) = 1985;

---How many employees joined each month in 1985?
SELECT 
    MONTH(Hire_Date) AS Join_Month, 
    COUNT(Employee_Id) AS Number_of_Employees 
FROM EMPLOYEE 
WHERE YEAR(Hire_Date) = 1985 
GROUP BY MONTH(Hire_Date);

---How many employees were joined in April 1985?
SELECT COUNT(Employee_Id) AS April_1985_Joinees 
FROM EMPLOYEE 
WHERE YEAR(Hire_Date) = 1985 AND MONTH(Hire_Date) = 4;

---Which is the Department ID having greater than or equal to 3 employees
--joining in April 1985?
SELECT Department_Id 
FROM EMPLOYEE 
WHERE YEAR(Hire_Date) = 1985 AND MONTH(Hire_Date) = 4 
GROUP BY Department_Id 
HAVING COUNT(Employee_Id) >= 3;

---List out employees with their department names.
SELECT 
    E.First_Name, 
    E.Last_Name, 
    D.Name AS Department_Name
FROM EMPLOYEE E
JOIN DEPARTMENT D ON E.Department_Id = D.Department_Id;

---Display employees with their designations.
SELECT 
    E.First_Name, 
    E.Last_Name, 
    J.Designation
FROM EMPLOYEE E
JOIN JOB J ON E.Job_Id = J.Job_ID;

---Display the employees with their department names and city.
SELECT 
    E.First_Name, 
    E.Last_Name, 
    D.Name AS Department_Name, 
    L.City
FROM EMPLOYEE E
JOIN DEPARTMENT D ON E.Department_Id = D.Department_Id
JOIN LOCATION L ON D.Location_Id = L.Location_ID;

---How many employees are working in different departments? Display with
--department names.
SELECT 
    D.Name AS Department_Name, 
    COUNT(E.Employee_Id) AS Number_of_Employees
FROM DEPARTMENT D
JOIN EMPLOYEE E ON D.Department_Id = E.Department_Id
GROUP BY D.Name;

---How many employees are working in the sales department?--
SELECT 
    COUNT(E.Employee_Id) AS Sales_Employees
FROM EMPLOYEE E
JOIN DEPARTMENT D ON E.Department_Id = D.Department_Id
WHERE D.Name = 'Sales';

---Which is the department having greater than or equal to 3
--employees and display the department names in
--ascending order.
SELECT 
    D.Name AS Department_Name
FROM DEPARTMENT D
JOIN EMPLOYEE E ON D.Department_Id = E.Department_Id
GROUP BY D.Name
HAVING COUNT(E.Employee_Id) >= 3
ORDER BY D.Name ASC;

---How many employees are working in 'Dallas'?
SELECT 
    COUNT(E.Employee_Id) AS Dallas_Employees
FROM EMPLOYEE E
JOIN DEPARTMENT D ON E.Department_Id = D.Department_Id
JOIN LOCATION L ON D.Location_Id = L.Location_ID
WHERE L.City = 'Dallas';

---Display all employees in sales or operation departments.
SELECT 
    E.First_Name, 
    E.Last_Name, 
    D.Name AS Department_Name
FROM EMPLOYEE E
JOIN DEPARTMENT D ON E.Department_Id = D.Department_Id
WHERE D.Name IN ('Sales', 'Operations');

---Display the employee details with salary grades. Use conditional statement to
--create a grade column.
SELECT 
    Employee_Id, First_Name, Last_Name, Salary,
    CASE 
        WHEN Salary >= 2500 THEN 'Grade A'
        WHEN Salary >= 1500 AND Salary < 2500 THEN 'Grade B'
        ELSE 'Grade C'
    END AS Salary_Grade
FROM EMPLOYEE;

---List out the number of employees grade wise. Use conditional statement to
--create a grade column.
SELECT 
    CASE 
        WHEN Salary >= 2500 THEN 'Grade A'
        WHEN Salary >= 1500 AND Salary < 2500 THEN 'Grade B'
        ELSE 'Grade C'
    END AS Salary_Grade,
    COUNT(Employee_Id) AS Number_of_Employees
FROM EMPLOYEE
GROUP BY 
    CASE 
        WHEN Salary >= 2500 THEN 'Grade A'
        WHEN Salary >= 1500 AND Salary < 2500 THEN 'Grade B'
        ELSE 'Grade C'
    END;

---Display the employee salary grades and the number of employees between
--2000 to 5000 range of salary.
SELECT 
    CASE 
        WHEN Salary >= 2500 THEN 'Grade A'
        WHEN Salary >= 1500 AND Salary < 2500 THEN 'Grade B'
        ELSE 'Grade C'
    END AS Salary_Grade,
    COUNT(Employee_Id) AS Number_of_Employees
FROM EMPLOYEE
WHERE Salary BETWEEN 2000 AND 5000
GROUP BY 
    CASE 
        WHEN Salary >= 2500 THEN 'Grade A'
        WHEN Salary >= 1500 AND Salary < 2500 THEN 'Grade B'
        ELSE 'Grade C'
    END;

 ---Display the employees list who got the maximum salary.
 SELECT * FROM EMPLOYEE 
WHERE Salary = (SELECT MAX(Salary) FROM EMPLOYEE);

---Display the employees who are working in the sales department.
SELECT * FROM EMPLOYEE 
WHERE Department_Id = (SELECT Department_Id FROM DEPARTMENT WHERE Name = 'Sales');

---Display the employees who are working as 'Clerk'.
SELECT * FROM EMPLOYEE 
WHERE Job_Id = (SELECT Job_ID FROM JOB WHERE Designation = 'Clerk');

---Display the list of employees who are living in 'Boston'.
SELECT * FROM EMPLOYEE 
WHERE Department_Id IN (
    SELECT Department_Id FROM DEPARTMENT WHERE Location_Id = (
        SELECT Location_ID FROM LOCATION WHERE City = 'Boston'
    )
);

---Find out the number of employees working in the sales department.
SELECT COUNT(Employee_Id) AS Sales_Employees 
FROM EMPLOYEE 
WHERE Department_Id = (SELECT Department_Id FROM DEPARTMENT WHERE Name = 'Sales');

---Update the salaries of employees who are working as clerks on the basis of
10%.
UPDATE EMPLOYEE 
SET Salary = Salary + (Salary * 0.10)
WHERE Job_Id = (SELECT Job_ID FROM JOB WHERE Designation = 'Clerk');

---Display the second highest salary drawing employee details.
SELECT * FROM EMPLOYEE 
WHERE Salary = (
    SELECT MAX(Salary) FROM EMPLOYEE 
    WHERE Salary < (SELECT MAX(Salary) FROM EMPLOYEE)
);

---List out the employees who earn more than every employee in department 30.
SELECT * FROM EMPLOYEE 
WHERE Salary > ALL (SELECT Salary FROM EMPLOYEE WHERE Department_Id = 30);

--Find out which department has no employees.
SELECT * FROM DEPARTMENT 
WHERE Department_Id NOT IN (SELECT DISTINCT Department_Id FROM EMPLOYEE WHERE Department_Id IS NOT NULL);
);

---Find out the employees who earn greater than the average salary for
--their department.
SELECT * FROM EMPLOYEE E1
WHERE Salary > (
    SELECT AVG(Salary) FROM EMPLOYEE E2 
    WHERE E1.Department_Id = E2.Department_Id
);
