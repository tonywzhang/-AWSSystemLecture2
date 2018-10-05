# AWS System Lecture 2

## Recap
You can scale up DB machine to many FB of RAM, but eventually need to scale out load.

By making database available on multiple machines, avoids several issues regarding machine failure. If one machine goes does, the entire system will not crash because the other machines have the same DB data available.

If multiple machines are connected to multiple databases, all of the data on all of the machines need to be updated with any changes to any database.

Even if a write is submitted to the leader database, the follower databases still need to update the data on their end. The followers will confirm that they've received and updated their database accordingly, the leader will confirm that the data has been written to all of the available databases.

The above action is asynchronous replication.

Splitting up the database into thirds allows the database to split up the partition of the database so that writes don't need to be written to every single DB. By splitting up those responsibilities, every leader can be responsible for their bit of the database.

PostgreSQL is able to reject any conflicting transactions. It is able to process transactions in parallel, but will also be able to stop any transactions that would cause any overlap issues.

Synchronous and Asynchronous calls are normally similar in terms of run speed, but can differ greatly if the machines that store the databases are extremely far in distance.

If there is only one leader database, it becomes the single point of failure. If it goes down, the app may crash. Promoting one follower to be a leader is more complicated than it might initially seem. There may be a ton of reconciliation processes that will run.


## Cats vs Dogs Tables

You could try to split cats up per ID, modding them by 4 and placing them into partitions accordingly. Doing so wouldn't change much in terms of finding the cat, but updating it would make it much faster, as it knows directly which partition that would contain the specific cat. The ability to scale out increases as the number of cats increases. By partitioning cats into different groups, it makes it easier to search into groups at a time.

## Users && Friendships Tables

If we have user with friend_id == 101, we can partition the friend_id's by mod 4, and then we can jump into partition 1, which should contain all of the friend_ids that return 1 when modded by 4.

It would allow us to scale our work, instead of the opposite scenario in which we need to join our data across all partitions, which is not scalable.

Instead of normalizing our data like we usually do, we can denormalize it by destroying our friendships joins table, and replacing the entirety of friend_id2s with the user information.

Normalization is the safer option of the two, but denormalization can have specific benefits if it will improve the run time and efficiency of your application.

## Review

Important to remove possibility of a single point of failure. If that one thing goes down, entire application goes down with it. Business ramifications include possibly losing millions of dollars.

Asynchronous log shipping is often less taxing than synchronous log shipping.

Partitioning the database is one way to make databases more efficient. By splitting up the database into multiple parts, the work is also split up and the load on the machine is that much less.

Sharding the data into separate parts only makes sense if you have multiple machines, to split and shoulder the load of holding the entire database. Sharding on one machine doesn't make much sense, as it would only complicate the issue.

Joins tables are queries across multiple tables, finding the joins relationships might require queries across all partitions, so partitions normally don't allow joins.

Problem with de-normalized data is that one change to a property is the time that it takes to change that one property every place that it is present. The write time performance is awful in that scenario. Extra space being used is another issue with de-normalized data.

In normalized data, you likely only need to change the property in one place. Much more efficient and practical.

If you think you're going to query for information more so than writing up information, de-normalizing the data makes sense. If there are more READ processes expected rather than POST requests, de-normalized data has a role.
