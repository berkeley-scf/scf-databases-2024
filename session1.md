---
author: Christopher Paciorek
layout: default
title: 'Introduction'
---

Topics:

- [Overview of databases](https://berkeley-scf.github.io/tutorial-databases/#3-database-systems-and-sql)
- [Working with large datasets: memory and disk](https://berkeley-scf.github.io/tutorial-databases/#2-background)
- [Database schema and normalization](https://berkeley-scf.github.io/tutorial-databases/#4-schema-and-normalization)

Discussion question:

I noticed a data analysis challenge (on Kaggle) involves analyzing data on motor vehicle collisions in New York City. Each record represents an individual collision, including the date, time and location of the accident, vehicles and victims involved, and factors contributing to the accident (e.g., weather, excessive speed, drunk driving, ...). Consider trying to set this up as a single table (or dataframe). What difficulties do you run into?

Now consider how you would create database containing multiple tables with this sort of information. What would the tables be? 

- [StackOverflow example database](https://berkeley-scf.github.io/tutorial-databases/#5-stack-overflow-example-database)

   The database we'll work with as our example is data on questions and answers on the Stack Overflow code discussion website from 2021. We don't have the full text of the questions or the answers, but we do have:
   
     - the question title and date posted,
     - the date posted for each answer,
     - some information about how popular the questions and answers were,
     - information about the users posting questions or answers, and
     - the topics (tags) associated with each question.

   Each question may have zero or more answers and each question has one or more tags indicating what the question is above.


- [Accessing a database from languages like R and Python](https://berkeley-scf.github.io/tutorial-databases/#6-accessing-a-database-and-using-sql-from-other-languages)

Recall that you can load the database into Python or R and see how to invoke an SQL query 

- [Basic SQL syntax](https://berkeley-scf.github.io/tutorial-databases/sql#1-introduction-to-sql)

Let's review the basic syntax (the "verbs") used in SQL by way of some examples.

In small groups, discuss what these queries do. Feel free to try running them if you're not sure or want to confirm your thoughts.

Note that for many of these, if you actually run them, you may want to add `limit 5` at the end of the query to limit how much output you get back.


### Select columns

```
select ownerid, title from questions
select ownerid, title from questions limit 5
select * from questions
select * from questions order by answercount desc
select count(*) as n from questions
select sum(answercount) from questions
```

### Using distinct

```
select distinct tag from questions_tags limit 15
select distinct ownerid, answercount from questions limit 15
select count(distinct tag) from questions_tags limit 15
```

### Filtering rows with `where`

```
select * from questions where answercount > 100
select * from questions where answercount > 100 order by answercount desc
select * from questions where answer = 100 limit 5
select * from questions_tags where tag like 'r-%' limit 10
select * from questions_tags where tag similar to 'r-%|%-r|r|%-r-%' limit 10
select * from questions_tags where tag in ('r','java','python') limit 10
```


