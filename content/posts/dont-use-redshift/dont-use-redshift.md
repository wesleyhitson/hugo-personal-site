---
title: "Don't Use Redshift"
date: 2024-10-24T21:49:56-04:00
draft: false
---
Several years ago, I took over partial data ownership a web application at work. The front end was a mishmash of Javascript and PHP, and the backend was C#. There were some areas for improvement there, but it was workable. However, the most glaring problem was the database. The entire application was backed by as single node Amazon Redshift database. I am here to spread the word:

### DON'T USE AMAZON REDSHIFT...

...as the only database for your entire modern web application. Just because your application is going to be so big that "big data!!1!" (spoiler...the data was not big) does not mean that a columnar database is going to serve the needs of a web application. Let me give a few reasons why.

#### 1. Redshift is a columnar database
As I mentioned, Redshift is a columnar database, sometimes called a data warehouse. That means that rather than storing data by rows, data is instead grouped by columns, which lends itself much better to aggregations and analytical queries. It also has good data compression, which is nice for reducing storage costs. However, what if, hypothetically, one of the main features of your application is letting users search by a particular field and pulling that specific record in? What if they want to do that 10 times in a session? 50? Are you *sure* you want to use Redshift for that? How will that affect the user experience when that search they want to do 10 times takes 20 seconds per query? I hope your loading icons look cool.

#### 2. No primary keys
Ok, this is a design decision, and I can understand why that decision might make sense for a columnar database (faster performance). However, the gripe I have is that Redshift will happily let you declare a column as a primary key, and it will tell you that it's a primary key, but it won't *enforce* it. Did I once spend 10 hours debugging something only to discover the root cause was a duplicate entry in a "primary key" column? Maybe. 

I understand that the query planner still uses that designation to help, so it's not entirely useless. However, I do feel like the phrase "primary key" means something very specific, and Redshift breaks that convention in a way that you will only know if you read deep into the documentation (which you will probably find at some point during your marathon debugging session). Maybe a different keyword for that type of column would be better. However, I am definitely not knowledgeable enough about the inner workings of databases and query planning to even begin to estimate the amount of effort it might take to change something like that, so maybe it was vastly easier to leave the name the same. 

#### 3. No indexes
I am not going to get into the nitty gritty here, but I do want to elaborate a little bit. Redshift, rather than allowing arbitrary indexes, lets you define a [distribution key](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-best-dist-key.html) and a [sort key](https://docs.aws.amazon.com/redshift/latest/dg/c_best-practices-sort-key.html). While you can define which columns those are, you cannot define an index, meaning that you cannot improve the performance of a slow query with an index. The issue comes when you're trying to do different things with the data in your web app, and you'd like to query the data in one way for one page, and another way for a different page. With a relational database, no sweat.

#### 4. Slow updates/writes
Because it's a columnar database, data is stored sequentially on the disk by column, rather than by row. While this means that aggregations across a column are quick, the tradeoff is that selecting all columns from a table is a slower operation because the data isn't sequential on the disk. And what if, purely hypothetically of course, a common operation was selecting one record from a table of 300M+ records? Or selecting just a few related records from a table of OVER ONE BILLION records? Yes, that has happened to me, and at a certain point, there's no way around it, that's how the web app has to work.

#### 5. Limited concurrency capabilities
Redshift has a concept of work queues, where it will determine which queries should execute and which ones should wait. However, this doesn't really help when a bunch of users all start using your application at the same time (8 am every day), and one big query can block out all other queries as they wait for it to finish. Then users get frustrated and think that maybe they didn't click the button hard enough, so they click it again, trying to get it to work, which backs up the queue even more. We had to scale the cluster out quite a bit to prevent the database from locking up every day, and even then there, were times we still had to manually troubleshoot locking events by manually killing database processes. And scaling out the cluster costs a not insignificant amount of money, as you have to scale in powers of 2.


Look, I'm not saying that you should never use Redshift. It's clearly a very powerful tool when used correctly (as a data warehouse). But you should understand its use case before you go picking the shiny new toy off the shelf instead of something that has been working very well for web apps for decades. If you really think that you are going to be doing aggregations accross your whole data set, then I would consider a hybrid approach where you have a Redshift database in addition to your relational database of choice. But save your sanity, and the sanity of those who will come after you, and don't write an entire web app backed only by Redshift.