# Data Normalization and Entity-Relationship Diagramming

## Entity - Relationship Diagram
I thought it would be useful to start drawing out the entity relationship diagram, as it would later allow us to directly change all tables in to fourth normal form (with the corresponding attributes). 

[Entity Relationship Diagram](images/Assignment_5_ERD.drawio.svg)

[Photo of Entity Relationship Diagram](images/ERD_Photo.png)

The process of converting this massive non-normalized table into fourth normal form started with me drawing out the Entity Relationship Diagram. I needed to see what nouns we were interested in (assignment, student, due date, professor, assignment topic, classroom, grade, relevant reading, and professor email) and whether they fell into the category of an attribute or an entity (depending on if we needed other entities to link to them). Butm there was a missing link of the section and courses entities that would draw the whole diagram together, so I started with the `Section` entity. I found that all the related entites would have a direct link with `Section`. Then I identified the cardinality between each entity. 

For example, I decided that one professor can teach many sections, but one section can only be taught by one professor.
One classroom can host many sections of classes, but each section can only be taught in one classroom. 
There are many sections for each course, but every section can only be of one course. 
One professor can assign many assignments, but one assignment can only be assigned by one professor (can be to different sections)
Hence all of the relationships above are one-to-many.

One section can be assigned many assignments, and one assignment can be assigned to many instructions.
One student can complete many assignments, and one student is completed by many assignments.
Finally, one student can be in many sections, and one section can hold many students. 
Hence all of the relationships above are many-to-many. 

Note: The relationship between professor and assignments can be negleted because we can access the professor and assignment table through the section table. However, if we would like to directly access the assignment table from professor, then it is easier to have that direct link.

Note: If we decide not to store any information about the attributes of a course, we can choose to make it an attribute of `Section`, but I am expecting a university to have some attributes that would be useful to describe a course. So I decided to make it an entity, but it really can be an attribute of `Section` if we were not interested in the details of `Course`. 



## Fourth Normal Form Compliant Tables

Original Table of the students' grades in courses at a university (already in first normal form)

| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   |
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |


### Reasons that don't make the table above Compliant to 4NF
(In order to be compliant to fourth normal form, the table needs to be compliant to 1st, 2nd, and 3rd normal forms).

First normal form is already satisfied via the presentation of the original table. 

Firstly, in order to be compliant to second normal form, all attributes must directly belong to the entity identified by the composite key of assignment_id and student_id. Hence in this table, the only attribute that does depend on the composite key (if we denote the composite key of this table to be `assignment_id` and `student_id`) is the attribute ` grades`. We need to know the `student_id` and `assignment_id` in order to find the grade associated to a student from a given assignment. The other attributes do not depend on the composite key, so this fails second normal form.

Secondly, the table fails third normal form because fails to comply with second normal form. In addition, the `professor email` attribute does not depend on any part of the composite key or the composite key as a whole. Rather, the `professor email` attribute depends completely on the `professor` attribute. Hence this does not satisfy third normal form.

The table does not comply to fourth normal form because the second and third normal forms are not compliant. 

### Tables in 4NF

Table 1: A table consisting of the Professor and its corresponding attributes

| Professor ID | Professor Name | Professor Email |
|-|-|-|
| 1 | Melvin | l.melvin@foo.edu |
| 2 | Logston | e.logston@foo.edu |
| 3 | Nevarez | i.nevarez@foo.edu | 
|...|...|...|

This table conforms to 1NF because there is a fixed schema and there is only one value per field per record.
This table conforms to 2NF because all the attributes describe the primary key `Professor ID` (and there are no composite key so 2ND cannot fail)
This table conforms to 3NF because all the attributes only describe the primary key directly and nothing else.
This table conforms to 4NF because there is only one independent multivalued column (which is the professor name).

Table 2: A table for the sections of all the classes 

|Section ID| Professor ID | Classroom ID | Section Name | Course ID |
|-|-|-|-|-|
| 1 | 1 | 1 | Section 1 | 1 |
| 2 | 2 | 2 | Section 2 | 1 |
| 3 | 3 | 2 | Section 3 | 1 |
|...|...|...|...|...|

This table conforms to 1NF because there is a fixed schema that all the data and there is only one value per field per record.
This table conforms to 2NF because all the attributes directly describe the primary key `Section ID` (no composite key, the other 3 IDs are foriegn keys that link to other tables).
This table conforms to 3NF because all the attributes only describe the primary key only. 
This table conforms to 4NF because there is only one independent multi-valued column (and the others depend on it)

Table 3: The Linking table between section and assignment (The Section-Assignment table)

|Section ID| Assignment ID| Due Date |
|-|-|-|
| 1 | 1 | 23.02.21 |
| 2 | 2 | 18.11.21 |
| 2 | 5 | 05.05.21 |
| 3 | 4 | 04.07.21 | 
|...|...|...|

This table conforms to 1NF because there is a fixed schema that all the data follow.
This table conforms to 2NF because all attributes describe the composite primary key of `Section ID` and `Assignment ID`.
This table conforms to 3NF because all attributes describe only instance represented by the composite primary key.
This table conforms to 4NF because there is only one independent multivalued attribute.

Table 4: Assignment Table that contains the assignments

|Assignment ID| Relevant Reading| Assignment Topic| Professor ID |
|-|-|-|
|1 | Deumlich Chapter 3| Data Normalization | 1 |
|2 | Dümmlers Chapter 11 | Single Table Queries | 2 |
|4 | Zehnder Page 87 | Spreadsheet Aggregate Functions | 3 |
|5 | Dümmlers Chapter 14 | Python and Pandas | 2 | 
|...|...|...| 

This table conforms to 1NF because there is a fixed schema that all data follow.
This table conforms to 2NF because there is no composite key, but all the data directly describe the instance represented by the priamry key.
This table conforms to 3NF because all the attributes describe the primary key and not other attributes.
This table conforms to 4NF because there is only one independent multivalued attribute. 

Table 5: The linking Table between Assignment and Student

|Assignment ID| Student ID| Grades |
|-|-|-|
|1|1| 80 |
|1|4|75|
|2|7| 25|
|4|2|65|
|5|2|92|
|...|...|...|

The table conforms to 1NF because there is a fixed schema that all data follow. 
This table conforms to 2NF because the attribute `Grades` describe the instance represented by the joint composite key of `Assignment ID` and `Student ID`.
This table conforms to 3NF because the attributes do not depend on other attributes.
THis table conforms to 4NF because there is only one independent attribute.

Table 6: The student table that includes all the relevant attributes

|Student ID| Section ID|
|-|-|
|1| 1 |
|2| 2 | 
|2| 3 |
|4| 1 |
|7| 2 |
|...|...|

The table conforms to 1NF because there is a fixed schema that follows.
This table conforms to 2NF because there is no composite key, and all attributes describe only the primary key `Student ID`
This table conforms to 3NF because all attributes only describe the identified instance and not other attributes.
This table conforms to 4NF because there is only one independent attribute.

Table 7: The Student-Section table is the linking table between student and section

|Section ID|Student ID|
|-|-|
|1| 1|
|1|4|
|2|7|
|2|2|
|3| 2| 
|...|...|

This table conforms to 1NF because there is a fixed schema that follows.
This table conforms to 2NF because all attributes describe the composite key of `Section ID` and `Student ID`
This table conforms to 3NF because all attributes describe the composite key and not other attributes.
This table conforms to 4NF because there are no other attributes, so there are no multi-valued fields.

Table 8: The table that contains the Courses of the school

|Course ID| Course Name | Course Description |
|-|-|-|
| 1 | Calculus 1 | Introduction to Calculus |
|...|...|...|


This table conforms to 1NF because the data follow a fixed schema and the fields all contain singular values.
This table conforms to 2NF because there are no composite keys, and all the attributes describe the instance identified by the priamry key. 
This table conforms to 3NF because all attributes describe the instance represented by the primary key and not other attributes.
This table conforms to 4NF because there is only one independent multi-valued attribute.

Table 9: The classrooms table.

|Classroom ID| Location|
|-|-|
|1| WWH 101|
|2| 60FA 314|
|...|...|

This table conforms to 1NF because all the data follow a fixed schema and all fields contain singular values.
This table conforms to 2NF because there are no composite keys, and all the attributes describe the instance identified by the primary key.
This table conforms to 3NF because all attributes describe the instance represented by the primary key and not other attributes.
This table conforms to 4NF because there is only one independent multi-valued attribute.
