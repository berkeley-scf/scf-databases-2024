Session 2:

#1

```
select distinct Q.questionid, Q.ownerid, Q.title from questions Q \
    join answers A using(questionid)

select * from questions \
    where questionid in \
    ( select questionid from answers )

select questionid from questions \
    intersect \
    select questionid from answers
```


#2 

```
select distinct Q.questionid, Q.ownerid, Q.title from questions Q left outer join answers A\
    using(questionid) where A.answerid is NULL

select * from questions \
    where questionid not in ( select questionid from answers )

select questionid from questions \
    except select questionid from answers
```


#3

```
select tag, count(*) as n from questions \
    join questions_tags using(questionid) \
    where questionid not in \
       ( select questionid from answers ) \
    group by tag order by n desc limit 10
```

```
select tag, count(*) as n from questions_tags join \
       ( select * from questions where questionid not in \
       (select questionid from answers) ) unanswered \
       using(questionid) \
       group by tag order by n desc limit 10
```
```
select tag, count(*) as n from questions_tags join \
       (select * from questions left outer join answers using(questionid) \
       where answerid is NULL) unanswered using(questionid) \
       group by tag order by n desc limit 10
```

Session 3:

Window function question

```
select * from questions join \
    ( select questionid, answerid, ownerid, rank() over \
    (partition by questionid order by reputation) as rank \
    from answers A join users U on A.ownerid = U.userid ) \
    using(questionid) where rank=1
```

Challenges

#1

```
select count(*) as n_users, avg_per_month from \
  (select ownerid, round(count(*)/12.0,1) as avg_per_month from questions \
     group by ownerid having ownerid not null) \
  group by avg_per_month order by avg_per_month limit 20
```

```
select count(*) as n_users, avg_per_month from \
  (select U.userid, round(count(Q.questionid)/12.0,1) as avg_per_month from users U \
  left outer join questions Q on Q.ownerid = U.userid group by U.userid) \
  group by avg_per_month order by avg_per_month limit 20
```

#2

```
select ownerid, tag, n from \
    ( select ownerid, tag, n, rank() over (partition by ownerid order by n desc ) as rank from \
    ( select ownerid, tag, count(*) as n from questions Q join questions_tags T using(questionid) \
        group by ownerid, tag) ) where rank <= 3
```

#3

```
select n_answers, avg(avg_per_month) as avg_user_q_per_month, count(*) as n_users from \
    ( select userid, n_answers, round(count(questionid)/12.0,1) as avg_per_month from \
        ( select userid, count(answerid) as n_answers from users left outer join \
        answers on ownerid = userid group by userid ) num_answers \
    left outer join questions \
    on ownerid=userid group by userid, n_answers ) \
  group by n_answers order by n_answers limit 20
```
