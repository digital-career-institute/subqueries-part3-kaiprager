# Table 1: JobOpenings
# Table 2: Applicants

![tablestructure](https://github.com/NootanVijapure/subqueries_part3/assets/30225165/72f6ba1d-dbca-4bc6-aa11-f34cbbf14fb3)

# data entry sample data
![dataentrytable1](https://github.com/NootanVijapure/subqueries_part3/assets/30225165/c91699b4-e1ba-4b55-bd64-f4ac7dbd3417)
![dataentrytable2](https://github.com/NootanVijapure/subqueries_part3/assets/30225165/c6250e16-afa8-402e-81ce-36e5d2c1e626)


## 1.List All Job Openings:
 Retrieve a list of all available job openings, including details about the title, department, minimum experience, and minimum education.
## 2. Applicants and Their Jobs:
Display the names of applicants and the jobs they have applied for. Include all applicants, even if they haven't applied for any job.
## 3. Qualified Applicants:
Create a report showing the names of applicants who meet or exceed the minimum experience and education requirements for the jobs they have applied for.
## 4. Job Applicants Without Match:
Identify applicants who have applied for a job that currently has no opening.
## 5.Experience Range Search:
Find all job openings that require a minimum of 3 to 5 years of experience.
## 6. Education Level Analysis:
Retrieve a list of job titles and the count of applicants who have applied for each job, grouped by education level (e.g., Bachelor's, Master's, PhD).



CREATE TABLE JobOpenings (JobID INT PRIMARY KEY, JobTitle VARCHAR(255), Department VARCHAR(255),
						 MinimumExperience INT, MinimumEducation VARCHAR(255));
						 
INSERT INTO JobOpenings VALUES
	(101, 'Spacecraft Pilot', 'Flight Operations', 5, 'Bachelor''s in Aerospace Engineering'),
	(102, 'Mission Specialist', 'Science', 3, 'PhD in Physics or related field'),
	(103, 'Space Engineer', 'Engineering', 3, 'Bachelor''s in Mechanical Engineering'),
	(104, 'Communications Officer', 'Communications', 2, 'Bachelor''s in Communications');
	
SELECT * FROM JobOpenings;

CREATE TABLE Applicants (ApplicantID INT PRIMARY KEY, ApplicantName VARCHAR(255), ExperienceYears INT,
						 Education VARCHAR(255), AppliedForJobID INT,
						 FOREIGN KEY(AppliedForJobID) REFERENCES JobOpenings(JobID));
						 
INSERT INTO Applicants VALUES
	(501, 'John Astronaut', 7, 'Master''s in Aerospace Engineering', 101),
	(502, 'Lisa Scientist', 4, 'PhD in Physics', 102),
	(503, 'Mark Engineer', 5, 'Bachelor''s in Mechanical Engineering', 103),
	(504, 'Emily Communicator', 3, 'Bachelor''s in Communications', 104);
	
SELECT * FROM Applicants;

SELECT JobTitle, Department, MinimumExperience, MinimumEducation FROM JobOpenings; 

SELECT j.JobTitle, a.ApplicantName FROM JobOpenings j
	RIGHT JOIN Applicants a ON j.JobID = a.AppliedForJobID;
	
SELECT a.ApplicantName, a.ExperienceYears, j.MinimumExperience FROM Applicants a
	JOIN JobOpenings j ON a.AppliedForJobID = j.JobID
	WHERE a.ExperienceYears >= j.MinimumExperience;
	
SELECT a.ApplicantID, a.ApplicantName, a.AppliedForJobID FROM Applicants a
	LEFT JOIN JobOpenings j ON a.AppliedForJobID = j.JobID
	WHERE j.JobID = NULL;
	
SELECT JobID, JobTitle, MinimumExperience FROM JobOpenings
	WHERE MinimumExperience >=3 AND MinimumExperience <= 5;
	
SELECT j.JobID, j.JobTitle, a.Education, COUNT(a.applicantID) AS ApplicantCount FROM JobOpenings j
	LEFT JOIN Applicants a ON j.JobID = a.AppliedForJobID
	GROUP BY j.JobID, a.Education;
