## General Notes

Try to use only 1 table. Even if it involves multiple types of items, each app or service should try to have only 1 dynamodb table.



## Difference between RDS and NoSQL
- In RDBMS, you design for flexibility without worrying about implementation details or performance. Query optimization generally doesn't affect schema design, but normalization is very important.
- In DynamoDB, you design your schema specifically to make the most common and important queries as fast and as inexpensive as possible. Your data structures are tailored to the specific requirements of your business use cases.