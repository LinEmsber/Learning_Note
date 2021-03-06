# ACID

## Introduction
In computer science, ACID (Atomicity, Consistency, Isolation, Durability) is a set of properties of database transactions. In the context of databases, a single logical operation on the data is called a transaction. For example, a transfer of funds from one bank account to another, even involving multiple changes such as debiting one account and crediting another, is a single transaction.


## Atomicity

Atomicity requires that each transaction be "all or nothing": if one part of the transaction fails, then the entire transaction fails (e.g. succeed-or-fail), and the database state is left unchanged.

An atomic system must guarantee atomicity in each and every situation, including power failures, errors, and crashes.


## Consistency

A transaction either creates a new and valid state of data, or, if any failure occurs, returns all data to its state before the transaction was started.

This does not guarantee correctness of the transaction in all ways the application programmer might have wanted (that is the responsibility of application-level code) but merely that any programming errors cannot result in the violation of any defined rules.

## Isolation
The isolation property ensures that the concurrent execution of transactions results in a system state that would be obtained if transactions were executed serially, i.e., one after the other. Providing isolation is the main goal of concurrency control. Depending on the concurrency control method , the effects of an incomplete transaction might not even be visible to another transaction.

## Durability
Committed data is saved by the system such that, even in the event of a failure and system restart, the data is available in its correct state.


## References
 - [ACID Wiki](https://en.wikipedia.org/wiki/ACID)
 - [ACID](http://searchsqlserver.techtarget.com/definition/ACID)
