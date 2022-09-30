# Student Directory 2 Tables Design Recipe

## 1. Extract nouns from the user stories or specification

```
As a coach
So I can get to know all students
I want to see a list of students' names.

As a coach
So I can get to know all students
I want to see a list of cohorts' names.

As a coach
So I can get to know all students
I want to see a list of cohorts' starting dates.

As a coach
So I can get to know all students
I want to see a list of students' cohorts.
```

```
Nouns:

student names, cohort names, cohort start dates
```

## 2. Infer the Table Name and Columns

Put the different nouns in this table. Replace the example with your own nouns.

| Record                | Properties          |
| --------------------- | ------------------  |
| cohort                | name, start date
| student               | name

1. Name of the first table (always plural): `cohorts` 

    Column names: `name`, `start_date`

2. Name of the second table (always plural): `students` 

    Column names: `name`

## 3. Decide the column types.

```
Table: cohorts
id: SERIAL
name: text
start_date: date

Table: students
id: SERIAL
name: text
```

## 4. Decide on The Tables Relationship

1. Can one cohort have many students? (Yes)
2. Can one student have many cohorts? (No)

You'll then be able to say that:

1. **[cohorts] has many [students]**
2. And on the other side, **[students] belongs to [cohorts]**
3. In that case, the foreign key is in the table [students]

```
-> Therefore, the foreign key is on the students table. (cohort_id)
```

## 4. Write the SQL.

```sql
-- EXAMPLE
-- file: cohorts_table.sql

-- Replace the table name, columm names and types.

-- Create the table without the foreign key first.
CREATE TABLE cohorts (
  id SERIAL PRIMARY KEY,
  name text,
  start_date date,
);

-- Then the table with the foreign key first.
CREATE TABLE students (
  id SERIAL PRIMARY KEY,
  name text,
-- The foreign key name is always {other_table_singular}_id
  cohort_id int,
  constraint fk_cohort foreign key(cohort_id)
    references cohorts(id)
    on delete cascade
);

```

## 5. Create the tables.

```bash
psql -h 127.0.0.1 database_name < albums_table.sql
```
