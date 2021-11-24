# Cloud Spanner

Google Cloud Spanner is an horizontally-scalable *managed* *relational* database.

It is an ACID-Compliant database. And it _does_ support schemas.

99.999% Availability (just a few minutes downtime per year).

It shards the data automatically.

## Use cases

- Financial
- Warehouse inventory

In general, the use case for Cloud Spanner is:

- When you need a _global_ database syncronized along the world.
- That is Consistent
- And you need to use Relational data

Without Cloud Spanner you have to create database clusters with replicas in different parts of the world.

## Cloud Spanner under the hood

Cloud Spanner is a "cloud-native" database.
This means the data and the query engine are designed to use efficiently the benefits that you get from using the cloud.
Storage and query engine are decoupled.
There is a storage layer that persist the data, and there is a query engine that gets the data from storage.
Both layers can grow as needed elastically.
In fact, the data layer will shard automatically the data - that is,
it will distribute the data several storage units to be able
to query that data in parallel in a transparent manner.


- [Video: Cloud Spanner Internals Part 1](https://youtu.be/nvlt0dA7rsQ)
- [Videp: Cloud Spanner Internals Part 2](https://www.youtube.com/zy-rcR4MoN4)


# How to use

With Cloud Spanner you have to provision instances. They are managed by Google, not by you.
The point of having Cloud Spanner instances is that you can control the throughtput of your database queries.

