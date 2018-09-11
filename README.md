# AWS System Lecture 2

## Recap
You can scale up DB machine to many FB of RAM, but eventually need to scale out load.

By making database available on multiple machines, avoids several issues regarding machine failure. If one machine goes does, the entire system will not crash because the other machines have the same DB data available.

If multiple machines are connected to multiple databases, all of the data on all of the machines need to be updated with any changes to any database.

Even if a write is submitted to the leader database, the follower databases still need to update the data on their end. The followers will confirm that they've received and updated their database accordingly, the leader will confirm that the data has been written to all of the available databases.

The above action is asynchronous replication.

Splitting up the database into thirds allows the database to split up the partition of the database so that writes don't need to be written to every single DB. By splitting up those responsibilities, every leader can be responsible for their bit of the database.

PostgreSQL is able to reject any conflicting transactions. It is able to process transactions in parallel, but will also be able to stop any transactions that would cause any overlap issues.
