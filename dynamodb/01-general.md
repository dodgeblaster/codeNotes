## General Notes

Try to use only 1 table. Even if it involves multiple types of items, each app or service should try to have only 1 dynamodb table.



## Difference between RDS and NoSQL
- In RDBMS, you design for flexibility without worrying about implementation details or performance. Query optimization generally doesn't affect schema design, but normalization is very important.
- In DynamoDB, you design your schema specifically to make the most common and important queries as fast and as inexpensive as possible. Your data structures are tailored to the specific requirements of your business use cases.

## The Primary Key
In a DynamoDB table, the primary key that uniquely identifies each item in the table can be composed not only of a partition key, but also of a sort key\.

Well\-designed sort keys have two key benefits:
+ They gather related information together in one place where it can be queried efficiently\. Careful design of the sort key lets you retrieve commonly needed groups of related items using range queries with operators such as `starts-with`, `between`, `>`, `<`, and so on\.
+ Composite sort keys let you define hierarchical \(one\-to\-many\) relationships in your data that you can query at any level of the hierarchy\.

  For example, in a table listing geographical locations, you might structure the sort key as follows:

  ```
  [country]#[region]#[state]#[county]#[city]#[neighborhood]
  ```

  This would let you make efficient range queries for a list of locations at any one of these levels of aggregation, from `country` all the way down to a `neighborhood`, and everything in between\.