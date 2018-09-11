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
