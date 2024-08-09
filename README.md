# Final-Project-on-Advanced-SQL-Techniques

This is a hands-on Project on Advanced SQL Techniques. 

# Software Used in this Lab
PgAdmin 4 (PostgreSQL 15)

# Objectives
The objectives of this project were to:

1. Use joins to query data from multiple tables

2. Create and query views

3. Write and run stored procedures

4. Use transactions

# Database Used in this Lab
The Three (3) datasets used in this project were gotten from the city of Chicago’s Data Portal:

Dataset 1 - Socioeconomic Indicators in Chicago

Dataset 2 - Chicago Public Schools

Dataset 3 - Chicago Crime Data

# Task A: Creating the Chicago database and the 3 datasets as tables in PgAdmin.

a. The new database named "ChicagoDB" was created in PgAdmin and the 3 tables were created by executing the following scripts rather than creating each table manually by typing the DDL commands in the SQL editor.

1.Chicago_public_schools_create_query_script: https://drive.google.com/file/d/114CoSCeFcSDzOLuS9FWUeIdY3iLKoN8A/view?usp=sharing

2.Chicago_crime_create_query_script: https://drive.google.com/file/d/1C3O8Aa47rka9L2LK0mQF5eQLvz3fXY7Z/view?usp=sharing

3.Chicago_socioeconomic_create_query_script: https://drive.google.com/file/d/1AoLeFsOOvERw1PNYBHmXUJQ18DxCqtx9/view?usp=sharing

b. After the 3 tables had been successfully created within the Chicago database, The 3 datasets were downloaded and imported into the already created tables as CSV files.

1. Chicago_public_schools_data - file can be downloaded here: https://drive.google.com/file/d/1vUT6RHg869rMTiy0ML-vWuyPW7fRu2m9/view?usp=sharing

2. Chicago_crime_data - file can be downloaded here: https://drive.google.com/file/d/1o30Anam0IZXfAFVZQwdxMDIKeW87YES_/view?usp=sharing

3. Chicago_socioeconomic_data - file can be downloaded here: https://drive.google.com/file/d/1FLKT0IhsQzxL89PIsRGv10bHCCl0qbVj/view?usp=sharing

To load each table, the following steps were taken:

1. Right-click on each table and select Import tab.

2. ChoOse the csv file and click OK 

3. The dataset was loaded

4. The above steps were repeated to load the other 2 datasets 

# Task B: Exploring the Tables

In exploring the tables within the database, the table was selected to view the First 100 Rows.

1. Chicago_public_schools_table

![image](https://github.com/user-attachments/assets/43cf21df-46b3-4075-bf29-d506438b351b)

2. Chicago_crime_table

![image](https://github.com/user-attachments/assets/ffdbaa69-b85e-415e-9f61-1c1cfb8314cc)

3. Chicago_socioeconomic_table

![image](https://github.com/user-attachments/assets/1bb622f2-b7ee-49ce-b0be-048f991b918b)

# Task C: Writing and excuting query commands to solve real-world problems

-1. Write and execute a SQL query to list the school names, community names and average attendance for communities with a hardship index of 98.

I tackled this problem using the following query statement:

          SELECT CP.name_of_school, CP.community_area_name, CP.average_student_attendance
          FROM chicago_public_schools AS CP
          LEFT JOIN chicago_socioeconomic_data AS CS
          ON CP.community_area_number = CAST(CS.community_area_number AS INTEGER)
          WHERE CS.hardship_index = '98';

![image](https://github.com/user-attachments/assets/0e2ddeb1-0256-4857-850b-8761657e43e4)


-2. Write and execute a SQL query to list all crimes that took place at a school. Include case number, crime type and community name.

I tackled this problem using the following query statement:

          SELECT CC.case_number, CC.primary_type, CS.community_area_name, CC.location_description
          FROM chicago_crime AS CC
          LEFT JOIN chicago_socioeconomic_data AS CS
          ON CC.community_area_number = CS.community_area_number 
          WHERE location_description LIKE '%SCHOOL%';

![image](https://github.com/user-attachments/assets/a4f320cd-f13d-40a4-b78d-bf72014b979a)

# Task B: Creating a View

For privacy reasons, create a view that enables users to select just the school name and the icon fields from the CHICAGO_PUBLIC_SCHOOLS table. By providing a view, you can ensure that users cannot see the actual scores given to a school, just the icon associated with their score. You should define new names for the view columns to obscure the use of scores and icons in the original table.

-1. Write and execute a SQL statement to create a view showing the columns listed in the following table, with new column names as shown in the second column.

![image](https://github.com/user-attachments/assets/406a466b-4259-4b98-82d0-0904a742434c)

To create a view showing the columns as listed in the above table, I issued the followwing query statement:

          CREATE VIEW chicago_public_schools_view (School_Name, Safety_Rating, Family_Rating, Environment_Rating, Instruction_Rating,	Leaders_Rating, Teachers_Rating)
          AS SELECT name_of_school, safety_icon, family_involvement_icon, environment_icon, instruction_icon, leaders_icon, teachers_icon	
          FROM chicago_public_schools; 

          SELECT * FROM chicago_public_schools_view 

![image](https://github.com/user-attachments/assets/110646f1-546c-4449-b91e-30e333343d54)

-2. Write and execute a SQL statement that returns just the school name and leaders rating from the view.

 To perform this task, I issued the followwing query statement:

          SELECT school_name, leaders_rating
          FROM chicago_public_schools_view 

![image](https://github.com/user-attachments/assets/bee76efa-f51c-4916-9edc-b9603aefeea6)

# Task C: Creating a Stored Procedure

The icon fields are calculated based on the value in the corresponding score field. You need to make sure that when a score field is updated, the icon field is updated too. To do this, you will write a stored procedure that receives the school id and a leaders score as input parameters, calculates the icon setting and updates the fields appropriately.

-1. Write the structure of a query to create or replace a stored procedure called UPDATE_LEADERS_SCORE that takes a in_School_ID parameter as an integer and a in_Leader_Score parameter as an integer.

To perform this task, I issued the followwing query statement:

          CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (
              in_School_ID INT,
              in_Leader_Score INT
          )
          LANGUAGE plpgsql
          AS $$
          BEGIN
              -- Add logic here (e.g., updating score and icon fields)
          END;
          $$;
          
![image](https://github.com/user-attachments/assets/6e6cbc8b-8d53-420d-afb8-39008a15a032)

-2. Inside your stored procedure, write a SQL statement to update the Leaders_Score field in the CHICAGO_PUBLIC_SCHOOLS table for the school identified by in_School_ID to the value in the in_Leader_Score parameter.

To perform this task, I issued the followwing query statement:

          CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (
              in_School_ID INT,
              in_Leader_Score INT
          )
          LANGUAGE plpgsql
          AS $$
          BEGIN
              -- Update the Leaders_Score field in the CHICAGO_PUBLIC_SCHOOLS table
              UPDATE CHICAGO_PUBLIC_SCHOOLS
              SET Leaders_Score = in_Leader_Score
              WHERE School_ID = in_School_ID;
          END;
          $$;

![image](https://github.com/user-attachments/assets/8487cedc-ea74-4a68-8409-c6082c7f6e0c)

-3. Inside your stored procedure, write a SQL IF statement to update the Leaders_Icon field in the CHICAGO_PUBLIC_SCHOOLS table for the school identified by in_School_ID using the following information.

![image](https://github.com/user-attachments/assets/7705b3b6-44b6-4875-98f9-df55751f6c0f)

To perform this task, I issued the followwing query statement:

          CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (
              in_School_ID INT,
              in_Leader_Score INT
          )
          LANGUAGE plpgsql
          AS $$
          BEGIN
              -- Update the Leaders_Score field
              UPDATE CHICAGO_PUBLIC_SCHOOLS
              SET Leaders_Score = in_Leader_Score
              WHERE School_ID = in_School_ID;

              -- Determine the appropriate icon based on the score
              IF in_Leader_Score BETWEEN 80 AND 99 THEN
                  UPDATE CHICAGO_PUBLIC_SCHOOLS
                  SET Leaders_Icon = 'Very strong'
                  WHERE School_ID = in_School_ID;
              ELSIF in_Leader_Score BETWEEN 60 AND 79 THEN
                  UPDATE CHICAGO_PUBLIC_SCHOOLS
                  SET Leaders_Icon = 'Strong'
                  WHERE School_ID = in_School_ID;
              ELSIF in_Leader_Score BETWEEN 40 AND 59 THEN
                  UPDATE CHICAGO_PUBLIC_SCHOOLS
                  SET Leaders_Icon = 'Average'
                  WHERE School_ID = in_School_ID;
              ELSIF in_Leader_Score BETWEEN 20 AND 39 THEN
                  UPDATE CHICAGO_PUBLIC_SCHOOLS
                  SET Leaders_Icon = 'Weak'
                  WHERE School_ID = in_School_ID;
              ELSE
                  UPDATE CHICAGO_PUBLIC_SCHOOLS
                  SET Leaders_Icon = 'Very weak'
                  WHERE School_ID = in_School_ID;
              END IF;
          END;
          $$;

![image](https://github.com/user-attachments/assets/5d212c86-2cd1-4c83-afe2-2b89b86b2ea5)

-4. Write a query to call the stored procedure, passing a valid school ID and a leader score of 50, to check that the procedure works as expected.

To perform this task, I issued the followwing query statement:

          CALL UPDATE_LEADERS_SCORE(1234, 50);

![image](https://github.com/user-attachments/assets/b1ee04d7-4563-4166-bd76-1ebaf28a10b7)

-5. Update your stored procedure definition. Add a generic ELSE clause to the IF statement that rolls back the current work if the score did not fit any of the preceding categories.

To perform this task, I issued the followwing query statement:

          CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (
              in_School_ID INT,
              in_Leader_Score INT
          )
          LANGUAGE plpgsql
          AS $$
          BEGIN
              -- Start a transaction block
              BEGIN
                  -- Update the Leaders_Score field
                  UPDATE CHICAGO_PUBLIC_SCHOOLS
                  SET Leaders_Score = in_Leader_Score
                  WHERE School_ID = in_School_ID;

                  -- Determine the appropriate icon based on the score
                  IF in_Leader_Score BETWEEN 80 AND 99 THEN
                      UPDATE CHICAGO_PUBLIC_SCHOOLS
                      SET Leaders_Icon = 'Very strong'
                      WHERE School_ID = in_School_ID;
                  ELSIF in_Leader_Score BETWEEN 60 AND 79 THEN
                      UPDATE CHICAGO_PUBLIC_SCHOOLS
                      SET Leaders_Icon = 'Strong'
                      WHERE School_ID = in_School_ID;
                  ELSIF in_Leader_Score BETWEEN 40 AND 59 THEN
                      UPDATE CHICAGO_PUBLIC_SCHOOLS
                      SET Leaders_Icon = 'Average'
                      WHERE School_ID = in_School_ID;
                  ELSIF in_Leader_Score BETWEEN 20 AND 39 THEN
                      UPDATE CHICAGO_PUBLIC_SCHOOLS
                      SET Leaders_Icon = 'Weak'
                      WHERE School_ID = in_School_ID;
                  ELSE
                      -- Roll back if the score is outside the valid range
                      RAISE EXCEPTION 'Invalid score: %', in_Leader_Score;
                  END IF;

                  -- Commit the transaction if everything is fine
                  COMMIT;
              EXCEPTION
                  WHEN OTHERS THEN
                      -- Roll back in case of any errors
                      ROLLBACK;
                      RAISE;
              END;
          END;
          $$;

![image](https://github.com/user-attachments/assets/373edaf5-66c5-4b2d-90e4-828b5091a675)

-6. Update your stored procedure definition again. Add a statement to commit the current unit of work at the end of the procedure.

To perform this task, I issued the followwing query statement:

         CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (
              in_School_ID INT,
              in_Leader_Score INT
          )
          LANGUAGE plpgsql
          AS $$
          BEGIN
              -- Update the Leaders_Score field
              UPDATE CHICAGO_PUBLIC_SCHOOLS
              SET Leaders_Score = in_Leader_Score
              WHERE School_ID = in_School_ID;

              -- Determine the appropriate icon based on the score
              IF in_Leader_Score BETWEEN 80 AND 99 THEN
                  UPDATE CHICAGO_PUBLIC_SCHOOLS
                  SET Leaders_Icon = 'Very strong'
                  WHERE School_ID = in_School_ID;
              ELSIF in_Leader_Score BETWEEN 60 AND 79 THEN
                  UPDATE CHICAGO_PUBLIC_SCHOOLS
                  SET Leaders_Icon = 'Strong'
                  WHERE School_ID = in_School_ID;
              ELSIF in_Leader_Score BETWEEN 40 AND 59 THEN
                  UPDATE CHICAGO_PUBLIC_SCHOOLS
                  SET Leaders_Icon = 'Average'
                  WHERE School_ID = in_School_ID;
              ELSIF in_Leader_Score BETWEEN 20 AND 39 THEN
                  UPDATE CHICAGO_PUBLIC_SCHOOLS
                  SET Leaders_Icon = 'Weak'
                  WHERE School_ID = in_School_ID;
              ELSE
                  -- Raise an exception for invalid scores
                  RAISE EXCEPTION 'Invalid score: %', in_Leader_Score;
              END IF;

              -- No explicit COMMIT or ROLLBACK needed
          END;
          $$;

![image](https://github.com/user-attachments/assets/7853dd50-3a19-4971-b927-1bbd1faef8d7)

-7. Write and run one query to check that the updated stored procedure works as expected when you use a valid score of 38.

To perform this task, I issued the followwing query statement:

          CALL UPDATE_LEADERS_SCORE(1, 38);

![image](https://github.com/user-attachments/assets/ce77264f-5612-414d-ae48-dd9075ed09f4)

8. Write and run another query to check that the updated stored procedure works as expected when you use an invalid score of 101.

To perform this task, I issued the followwing query statement:

          -- Call the stored procedure with an invalid score of 101
          DO $$
          BEGIN
              BEGIN
                  -- Attempt to call the procedure with an invalid score
                  CALL UPDATE_LEADERS_SCORE(1, 101);
              EXCEPTION
                  WHEN others THEN
                      -- Handle the exception and print a message
                      RAISE NOTICE 'Caught exception: %', SQLERRM;
              END;
          END;
          $$;

          -- Query to check if the updates were applied correctly or if they were rolled back
          SELECT School_ID, Leaders_Score, Leaders_Icon
          FROM CHICAGO_PUBLIC_SCHOOLS
          WHERE School_ID = 1;

![image](https://github.com/user-attachments/assets/22846d7c-4960-4362-8022-256643f65314)


