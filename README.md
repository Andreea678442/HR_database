# Database Project for **HR**
The scope of this project is to use all the SQL knowledge gained throught the Software Testing course and apply them in practice.

## Application under test: HR

### Tools used: MySQL Workbench

**Database description**:  HR_Database is an application for the management of human resources in a company. It provides functionality for managing employee information, performance appraisals, training programs. Through statistics and reports contributing to the improvement of employee evaluation and development processes.

#### Database Schema
You can find below the database schema that was generated through Reverse Engineer and which contains all the tables and the relationships between them.

![image](https://github.com/user-attachments/assets/b567a5fe-13d7-4c4a-9f20-2f479e6dd01d)


The tables are connected in the following way:

* **POZITII** is connected with **ANGAJATI** through a **many-to-one** relationship which was implemented through **pozitii.id_pozitie** as a primary key and **angajati.pozitie_id** as a foreign key
* **DEPARTAMENTE** is connected with **ANGAJATI** through a **many-to-one** relationship which was implemented through **departamente.id_departament** as a primary key and **angajati.departament_id** as a foreign key
* **ANGAJATI** is connected with **EVALUARI_ANGAJATI** through a **one-to-many** relationship which was implemented through **angajati.id_angajat** as a primary key and **evaluari_angajati.angajat_id** as a foreign key
* **DEZVOLTARE** is connected with **ANGAJATI_TRAINING** through a **one-to-many** relationship which was implemented through **angajati.id_angajat** as a primary key and **angajati_training.angajat_id** as a foreign key
* **ANGAJATI** is connected with **DEZVOLTARE** through a **many-to-many** relationship which was implemented through **angajati.id_angajat** as a primary key and **angajati_training.angajat_id** as a foreign key and dezvoltare.id_training as a primary key and angajati_training.training_id as a foreign key.

#### Primary key
Primary key is a field in a table that uniquely identifies each row or record in a table. A primary key column cannot have a null value, it must contain s value for every row. MySQL automatically creates a unique index for the primary key, which helps in efficiently retrieving records.

#### Foreign key
Foreign key in MySQL is a column (or a set of columns) in one table that refers to the primary key in another table. It is used to establish and enforce a link between the data in two tables, ensuring referential integrity. This means that the foreign key ensures that the values in the foreign key column match the values in the referenced table's primary key, or are NULL.

### __Database Queries__

__DDL (Data Definition Language)__
The following instructions were written in the scope of CREATING the structure of the database (CREATE INSTRUCTIONS)

```create database HR;```
```use HR;```
```
create table ANGAJATI(
id_angajat int primary key auto_increment,
nume varchar(15),
prenume varchar(15),
departament_id int,
foreign key (departament_id) references DEPARTAMENTE(id_departament),
data_angajarii date,
salariu int
);
```
```
create table DEPARTAMENTE (
id_departament int primary key auto_increment,
nume_departament varchar(10),
locatie varchar(10)
);
```
```create table EVALUARI_ANGAJATI (
id_evaluare int primary key auto_increment,
angajat_id int,
foreign key (angajat_id) references ANGAJATI(id_angajat),
scor decimal (3.1),
comentarii text
);
```
```
create table DEZVOLTARE (
id_training int primary key auto_increment,
titlu varchar(150),
data_training date
);
```
```
create table ANGAJATI_TRAINING (
id_angajati_training int primary key auto_increment,
angajat_id int,
foreign key (angajat_id) references ANGAJATI(id_angajat),
training_id int,
foreign key (training_id) references DEZVOLTARE(id_training),
status ENUM('finalizat', 'in desfășurare', 'anulat')
);
```
After the database and the tables have been created, a few __ALTER instructions__ were written in order to update the structure of the database, as described below:

- define a new column:
<code> alter table ANGAJATI add column pozitie_id int;</code>
<code> alter table DEZVOLTARE add column angajat_id int;</code>
- add 2 new column in a table:<code> ALTER TABLE angajati ADD COLUMN email VARCHAR(50), ADD COLUMN numar_telefon varchar(10);</code>
- add a column as a foreign key:<code> ALTER TABLE angajati ADD CONSTRAINT fk_angajati FOREIGN KEY (pozitie_id) REFERENCES POZITII(id_pozitie); </code>
<code>ALTER TABLE dezvoltare ADD CONSTRAINT fk_dezvoltare FOREIGN KEY (angajat_id) REFERENCES angajati(id_angajat);</code>
- change column property: <code>alter table evaluari_angajati</code> <code>modify column scor decimal(3,2);</code> <code>alter table angajati modify numar_telefon varchar(15);</code>

__DML (Data Manipulation Language)__
In order to be able to use the database I populated the tables with various data necessary in order to perform queries and manipulate the data. In the testing process, this necessary data is identified in the Test Design phase and created in the Test Implementation phase.

Below you can find all the insert instructions that were created in the scope of this project:

- insert into all the columns-multiple rows:
```
insert into DEPARTAMENTE (nume_departament, locatie)
values 
('HR', 'Bucuresti'),
('IT', 'Cluj'),
('Vanzari', 'Timisoara'),
('Logistica','Timisoara');
```
```
insert into POZITII (categorie_pozitie)
values
('management'),
('team');
```
```
insert into ANGAJATI (nume, prenume, departament_id, data_angajarii, salariu, pozitie_id)
values
('Popescu', 'Ion', 1, '2022-01-15', 7000, 1),
('Ionescu', 'Maria', 1, '2023-05-20', 4000, 2),
('Vasilescu', 'Andrei', 2, '2020-05-29', 8000, 1),
('Marinescu', 'Eugenia', 2, '2021-09-10', 5000, 2),
('Diaconu', 'Ioana', 3, '2020-06-18', 9500, 1),
('Munteanu', 'Dorel', 3, '2020-08-06', 5500, 2),
('Georgescu', 'Daniela', 4, '2022-06-15', 3000, 2),
('Iliescu', 'Ionut', 4, '2022-06-20', 3000, 2);
```
```
insert into DEZVOLTARE (titlu, data_training)
values
('Leadership & Coaching Course', '2024-01-13'),
('Sales & Negotiation Techniques Course', '2024-02-28'),
('Business Communication Skills Course', '2024-03-09'),
('Strategical & Critical Thinking Course','2024-04-17'),
('Business Resillience Course','2024-05-31'),
('Artificial Intelligence Course', '2024-06-20'),
('Mobile Development Course', '2024-07-22'),
('Digital Transformation Course','2024-08-16');
```
```
insert into evaluari_angajati (angajat_id, scor, comentarii)
values
('1', 4.5, 'A demonstrat abilități excelente în gestionarea proiectelor. A respectat termenele limită și a colaborat eficient cu echipa. Ar putea să îmbunătățească comunicarea cu alte departamente pentru a facilita fluxul de informații.'),
('6', 4.8, 'Are cunoștințe tehnice solide! Cu puțin mai multă atenție la gestionarea timpului, va putea să performeze și mai bine.'),
('2', 4.8, 'A fost un mentor valoros pentru colegi. Sfatul ar fi să-și împărtășească experiențele mai des, pentru a ajuta la dezvoltarea profesională a altora.'),
('3', 4.1, 'Are potențial, dar întâmpină dificultăți în respectarea termenelor.'),
('4', 4.4, 'Își finalizează sarcinile, dar ar trebui să fie mai proactiv în a aduce idei noi. O atitudine mai deschisă la feedback'),
('5', 4.6, 'Oferă un leadership excelent și este întotdeauna dispusă să ajute colegii. Ar fi bine să își împărtășească mai des cunoștințele.');
```
```
insert into ANGAJATI_TRAINING (angajat_id, training_id, status)
values
('1','3','finalizat'),
('1','4','finalizat'),
('5','2','in desfasurare'),
('6','2','finalizat'),
('5','3','finalizat'),
('6','3','finalizat'),
('3','6','in desfasurare'),
('4','7','finalizat'),
('3','7','in desfasurare'),
('4','6','anulat'),
('2','4','anulat');
```
- insert into all the columns-one row only:
```
insert into angajati values
('9', 'Diaconescu', 'Marian', 3, '2022-11-01', 4000, 2, 'diaconescumarian@companie.com', '0125456456');
```
- insert made for certain column:
```
insert into dezvoltare (titlu) values ('Business English Language Training');
```

After the insert, in order to prepare the data to be better suited for the testing process, I updated some data in the following way:

- employee salary update to 6500 from 6000
```
UPDATE ANGAJATI
SET salariu = 6500
WHERE id_angajat = 1;
```

- 10% salary increase for IT department employees
```
UPDATE ANGAJATI
SET salariu = salariu * 1.1 
WHERE departament_id = 2;
```
- after adding a new column in the employee table, populated those cells with new value :
```
UPDATE angajati
SET 
    email = CASE 
                  WHEN id_angajat = 1 THEN 'popescuion@companie.com'
                  WHEN id_angajat = 2 THEN 'ionescumaria@companie.com'
                  WHEN id_angajat = 3 THEN 'vasilescuandrei@companie.com'
                  WHEN id_angajat = 4 THEN 'marinescueugenia@companie.com'
                  WHEN id_angajat = 5 THEN 'diaconuioana@companie.com'
                  WHEN id_angajat = 6 THEN 'munteanudorel@companie.com'
                  WHEN id_angajat = 7 THEN 'georgescudaniela@companie.com'
                  WHEN id_angajat = 8 THEN 'iliescuionut@companie.com'
                  END,
numar_telefon = CASE 
                  WHEN id_angajat = 1 THEN '1234567890'
                  WHEN id_angajat = 2 THEN '0987654321'
                  WHEN id_angajat = 3 THEN '1122334455'
                  WHEN id_angajat = 4 THEN '2233445566'
                  WHEN id_angajat = 5 THEN '3344556677'
                  WHEN id_angajat = 6 THEN '4455667788'
                  WHEN id_angajat = 7 THEN '5566778899'
                  WHEN id_angajat = 8 THEN '6677889900'
              
              END
WHERE id_angajat IN (1, 2, 3, 4, 5, 6, 7, 8);
```

__DQL (Data Query Language)__
After the testing process, I deleted the data that was no longer relevant in order to preserve the database clean:

- delete a table column - this column was not used in the end, hence it is not part of the database anymore
<code>alter table angajati_training drop column id_angajati_training;</code>

- deleted some entries as I inserted the data twice:  <code> delete from departamenteWHERE id_departament = 5;</code>

In order to simulate various scenarios that might happen in real life I created the following queries that would cover multiple potential real-life situations:

- __WHERE__ - searched for the people in the employees table whose second name is "Diaconescu"
```
select * from angajati where nume = "Diaconescu";
```
- __AND__ - select all HR employees who have a salary > 5000
```
select nume, prenume, salariu from angajati where departament_id = 1 and salariu > 5000;
```
- __OR__ - select the departments that are either in "Bucharest" or have the location "Timisoara"
```
select nume_departament, locatie from departamente where locatie = 'Bucuresti' or locatie = 'Timisoara';
```
-__LIKE__ - search for the trainings from March 
```
select * from dezvoltare where data_training like '%-03-%';
```
#### Joins

-_INNER JOIN select every department employee, the people who have associated a departments to their name_
```
select A.nume, A.prenume, D.nume_departament
from angajati A
inner join departamente D on A.departament_id = D.id_departament;
```
_LEFT JOIN select all employees and their ratings_
```
select A.nume nume_angajat, A.prenume prenume_angajat, E.scor, E.comentarii
from angajati A
left join evaluari_angajati E ON A.id_angajat = E.angajat_id;
```
-_RIGHT JOIN returns which job position the employees have_
```
select P.categorie_pozitie, A.nume, A.prenume
from angajati A
right join pozitii P ON A.pozitie_id = P.id_pozitie;
```

#### Aggregate functions<br>
__AVG__ returns the average score of all employees
```
select avg (scor) from evaluari_angajati;
```
__GROUP BY__ & __HAVING__ returns employees who have an above-average score on EMPLOYEE EVALUATION
```
select 
    a.id_angajat,
    a.nume,
    a.prenume,
    avg(e.scor) as scor_mediu
from 
    angajati a
join 
    evaluari_angajati e on a.id_angajat = e.angajat_id
group by 
    a.id_angajat
having 
    avg(e.scor) > (select avg(scor) from evaluari_angajati);
```
__MIN__ or __MAX__
```
SELECT min(salariu) FROM angajati;
SELECT max(salariu) FROM angajati
```
__ORDER BY__ order the employees in ascending order according to the date of employment
```
SELECT * FROM angajati ORDER BY data_angajarii ASC;
```

#### Subqueries

**subquery** returns all employees who do not have evaluations performed, by querying the ANGAJATI table, then the subquery checks if there are ratings for each employee in the EVALUARI_ANGAJATI table
```
select 
    A.id_angajat,
    A.nume,
    A.prenume
from 
    ANGAJATI A
where 
    not exists (select 1 
                 from EVALUARI_ANGAJATI E 
                  where E.angajat_id = A.id_angajat);
```

# Conclusions
Working on the HR project I realized how important it is to organize the data in a correct structure that can make the difference in the efficiency of an application. I discovered how powerful a database can be in handling large volumes of information and the queries can provide clear and quick returns to complex questions.<br>
I am convinced that all this knowledge and experience will be extremely useful to me in future projects.
