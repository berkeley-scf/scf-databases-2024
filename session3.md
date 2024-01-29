---
author: Christopher Paciorek
layout: default
title: 'Window functions and complicated queries'
---

## Window functions

- [Window functions](https://berkeley-scf.github.io/tutorial-databases/sql#33-window-functions)

In small groups, work on this window-function-based question.

For each question find the answer from the user with the maximum reputation
amongst all those answering the question.

 - Start by determining the rank by reputation of the ownerid amongst all answers.
 - Then bring in the question information.
 - Then restrict to the highest rank.


## Challenges

These are based on [some sample questions](https://berkeley-scf.github.io/tutorial-databases/sql#34-putting-it-all-together-to-do-complicated-queries) suggested to me by a data science manager from one of the big tech companies.

Let's work on them in small groups.

1) For each user who has asked at least one question find the average number of questions per month. Then determine the distribution of that average across the users. (I.e., determine the values that would go into a histogram of the average number of questions per month across users.) The output should be something like:

```
number of questions per month (rounded) | number of users
```

Next try to figure out how to include the users who have asked no questions. Hint: think about how to get NULL values included and then count a column containing such values.


2) For each user, find the three most common tags they apply to their questions.

The output should be something like:

```
userid | tag | count
```

Hints: You'll need to use subqueries and the final selection of just the top 3 tags will need to be done in its own query and not as part of defining a field using a window function.


3) Consider trying to determine whether users who answer a lot of questions also ask a lot of questions. Grouping the users based on the number of questions they've answered (0, 1, 2, etc.), determine the aerage number of questions per month for each group. 

The output should be something like:

```
number of answers | average number of questions per month
```

You'll want to work through this in pieces. Try to think about the initial tables you would need and then build up your query in a nested fashion.


