## Student Database Management and Querying: An In-Depth Look at `student_info.sh` and `students.sql`

This article delves into a GitHub repository containing two key files: `students.sql` and `student_info.sh`. Together, these files provide a robust system for managing and querying a PostgreSQL database dedicated to student information in a computer science department.

### The Foundation: `students.sql` – Database Schema and Initial Data

The `students.sql` file serves as the blueprint for the PostgreSQL database. It meticulously defines the database schema and populates it with initial data, ensuring a ready-to-use environment for student record management.

**Database and Table Creation:**
The script begins by ensuring a clean slate, dropping the `students` database if it already exists, and then creating a new one with `UTF8` encoding and specific locale settings.

The core of the database is structured around four interconnected tables:

  * **`courses`**: This table stores information about various computer science courses.

      * `course_id`: A unique integer identifier for each course, automatically incremented.
      * `course`: A string (up to 100 characters) representing the name of the course.

  * **`majors`**: This table lists the different majors offered within the computer science program.

      * `major_id`: A unique integer identifier for each major, automatically incremented.
      * `major`: A string (up to 50 characters) for the major's name.

  * **`majors_courses`**: This is a junction table that establishes a many-to-many relationship between `majors` and `courses`. It defines which courses are part of which majors.

      * `major_id`: Foreign key referencing `majors.major_id`.
      * `course_id`: Foreign key referencing `courses.course_id`.

  * **`students`**: This central table holds the individual student records.

      * `student_id`: A unique integer identifier for each student, automatically incremented.
      * `first_name`: The student's first name (up to 50 characters).
      * `last_name`: The student's last name (up to 50 characters).
      * `major_id`: A foreign key referencing `majors.major_id`, indicating the student's declared major. This field can be `NULL` if a student has not yet selected a major.
      * `gpa`: The student's Grade Point Average, a numeric value with 2 digits before and 1 digit after the decimal point. This field can also be `NULL`.

**Initial Data Population:**
The `students.sql` file also pre-populates these tables with sample data, providing a functional dataset for testing and demonstration purposes. This includes a diverse set of courses (e.g., 'Data Structures and Algorithms', 'Web Programming', 'Machine Learning'), various majors (e.g., 'Database Administration', 'Web Development', 'Data Science'), associations between majors and courses, and a list of students with their names, majors (or lack thereof), and GPAs.

### The Interface: `student_info.sh` – Querying Student Data

The `student_info.sh` script is a bash script designed to interact with the PostgreSQL database established by `students.sql`. It uses `psql` commands to execute specific SQL queries and present the results in a user-friendly format. The script is structured to provide various insights into the student data.

**Key Queries and Functionality:**

  * **Students with a 4.0 GPA:** This query retrieves the first name, last name, and GPA of all students who have achieved a perfect 4.0 GPA.
    ```bash
    SELECT first_name, last_name, gpa FROM students WHERE gpa = 4.0
    ```
  * **Courses Alphabetically Before 'D':** The script lists all course names whose first letter comes before 'D' in the alphabet.
    ```bash
    SELECT course FROM courses WHERE course < 'D'
    ```
  * **Students with Specific Last Name and GPA Criteria:** This more complex query identifies students whose last name starts with 'R' or later in the alphabet, and whose GPA is either greater than 3.8 or less than 2.0.
    ```bash
    SELECT first_name, last_name, gpa FROM students WHERE last_name >= 'R' AND (gpa > 3.8 OR gpa < 2.0)
    ```
  * **Last Names with 'sa' or 'r' as Second to Last Letter:** This query demonstrates case-insensitive pattern matching (`ILIKE`) to find last names containing "sa," or `LIKE` for last names with 'r' as the second-to-last character.
    ```bash
    SELECT last_name FROM students WHERE last_name ILIKE '%sa%' OR last_name LIKE '%r_'
    ```
  * **Students Without a Major and Specific First Name/GPA:** This query targets students who have not yet declared a major (`major_id IS NULL`) and either their first name begins with 'D' or their GPA is greater than 3.0.
    ```bash
    SELECT first_name, last_name, gpa FROM students WHERE major_id IS NULL AND (first_name LIKE 'D%' OR gpa > 3.0)
    ```
  * **(Partially Implemented) Course Names with 'e' as Second Letter or Ending with 's':** The script includes a placeholder for a query that would retrieve the first five course names, in reverse alphabetical order, that have an 'e' as the second letter or end with an 's'. This indicates a potential area for further development and refinement of the script's capabilities.

### Conclusion

The combination of `students.sql` and `student_info.sh` provides a practical example of how to set up and interact with a PostgreSQL database for managing educational data. The `students.sql` file lays the groundwork with a well-defined schema and initial data, while `student_info.sh` showcases various useful queries that can be performed to extract meaningful insights from the student records. This repository serves as a valuable resource for anyone looking to understand basic database design, SQL querying, and shell scripting for database interaction.
