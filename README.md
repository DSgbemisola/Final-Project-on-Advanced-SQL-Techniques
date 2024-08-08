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

 To pwerform this task, I issued the followwing query statement:

          SELECT school_name, leaders_rating
          FROM chicago_public_schools_view 

![image](https://github.com/user-attachments/assets/bee76efa-f51c-4916-9edc-b9603aefeea6)

# Task C: Creating a Stored Procedure

The icon fields are calculated based on the value in the corresponding score field. You need to make sure that when a score field is updated, the icon field is updated too. To do this, you will write a stored procedure that receives the school id and a leaders score as input parameters, calculates the icon setting and updates the fields appropriately.

-1. Write the structure of a query to create or replace a stored procedure called UPDATE_LEADERS_SCORE that takes a in_School_ID parameter as an integer and a in_Leader_Score parameter as an integer.

To pwerform this task, I issued the followwing query statement:

          CREATE OR REPLACE PROCEDURE UPDATE_LEADERS_SCORE (
                      in_School_ID INT,
                      in_Leader_Score INT
                    )
          BEGIN
    --Update the score field
          UPDATE school_table
          SET leader_score = in_Leader_Score
          WHERE school_id = in_School_ID;
    -- Calculate the icon based on the updated score
          DECLARE v_Icon VARCHAR(50);
          IF in_Leader_Score >= 90 THEN
          SET v_Icon = 'Gold';
       ELSIF in_Leader_Score >= 75 THEN
          SET v_Icon = 'Silver';
          ELSIF in_Leader_Score >= 50 THEN
          SET v_Icon = 'Bronze';
          ELSE
        SET v_Icon = 'No Badge';
        END IF;          
    -- Update the icon field
        UPDATE school_table
              SET leader_icon = v_Icon
              WHERE school_id = in_School_ID;

          END;

