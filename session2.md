---
author: Christopher Paciorek
layout: default
title: 'Core SQL functionality'
---

In this session, we'll work through grouping/aggregation/reduction, joins (including self-joins), set operations, and subqueries.

## Group by

- [Grouping/stratification](https://berkeley-scf.github.io/tutorial-databases/sql#13-grouping--stratifying-group-by)

In small groups, discuss what these queries do.

```
select tag, count(*) as n from questions_tags \
    group by tag

select tag, count(*) as n from questions_tags \
    group by tag having n > 1000
    
select ownerid, count(*) as n from questions \
    group by ownerid order by n desc limit 15
    
select ownerid, sum(viewcount) as viewed from questions \
    group by ownerid

select *, sum(viewcount) as viewed from questions \
    group by ownerid

select answercount, commentcount, count(*) as n from questions \
    group by answercount, commentcount

select tag, count(*) as n from questions_tags \
    where tag like 'python%' group by tag having n > 1000
```

The query above starting with `select *, sum(viewcount)` behaves differently in SQLite and DuckDB.

## Joins

- [Merging tables (joins)](https://berkeley-scf.github.io/tutorial-databases/sql#14-joins)

In small groups, discuss what these queries do.

### Inner joins

```
select * from questions join questions_tags \
    on questions.questionid = questions_tags.questionid
    
select * from questions Q join questions_tags T \
    on Q.questionid = T.questionid
    
select * from questions Q join questions_tags T \
    using(questionid)
    
select * from questions Q, questions_tags T \
    where Q.questionid = T.questionid
```

### Outer joins

```
select * from questions Q left outer join answers A \
    on Q.questionid = A.questionid 
    
select * from questions Q left outer join answers A \
    on Q.questionid = A.questionid \
    where A.creationdate is NULL
    
# Note no right outer join in SQLite so here we reverse order of answers and questions \
select * from questions Q right outer join answers A \
    on Q.questionid = A.questionid \
    where Q.creationdate is NULL

select Q.questionid, count(*) as n_tags from questions Q join questions_tags T \
    on Q.questionid = T.questionid \
    group by Q.questionid
```

### Self joins

First we'll set up a view (a temporary) table that combines questions and tags for ease of illustrating ideas around self joins.

```
create view QT as select * from questions join questions_tags using(questionid)
```

In small groups, discuss what these queries do.

```
select * from QT as QT1 join QT as QT2 \
    using(questionid)

select * from QT as QT1 join QT as QT2 \
    using(questionid) where QT1.tag < QT2.tag
    
select QT1.tag, QT2.tag, count(*) as n from QT as QT1 join QT as QT2 \
    using(questionid) where QT1.tag < QT2.tag \
    group by QT1.tag, QT2.tag order by n desc limit 10


select * from QT as QT1 join QT as QT2 using(ownerid)
```

### Set operations


- [Set operations](https://berkeley-scf.github.io/tutorial-databases/sql#31-set-operations-union-intersect-except)

In small groups, discuss what these queries do.

```
select ownerid from QT where tag='python' \
    intersect \
    select ownerid from QT where tag='r'
    
select ownerid from QT where tag='python' \
    except \
    select ownerid from QT where tag='r'
    
select ownerid from QT where tag='python' \
    union \
    select ownerid from QT where tag='r'

select userid, displayname, location from users \
    where location like '%United States%' \
    union \
    select userid, displayname, location from users \
    where location like '%Canada%'
```

### Subqueries

In small groups, discuss what these queries do.

- [Subqueries (and with statements)](https://berkeley-scf.github.io/tutorial-databases/sql#32-subqueries)

```
select * from \
    answers A \
    join \
    ( select ownerid, count(*) as n_answered from answers \
        group by ownerid order by n_answered desc limit 1000 ) most_responsive \
    on A.ownerid = most_responsive.ownerid
```


```
select avg(upvotes) from users \
    where userid in \
    ( select distinct ownerid from \
    questions join questions_tags using(questionid) \
    where tag = 'python' )
```

## Challenges: Joins, set operations, grouping and subqueries

1. Find all the questions that have answers using:
   - a join
   - a subquery
   - a set operation

2. Find all the questions that have no answers using:
   - a join
   - a subquery
   - a set operation

3. Find the 10 most popular tags for unanswered questions.
There are a variety of ways to do this using subqueries.
Feel free to start by figuring out how to do it using views.


