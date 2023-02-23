## Not in : One null value is enough to cause your query to return no results.
- hive> select * from A where A.id not in (select id from B where id is not null);
- NOT IN in the WHERE clause with uncorrelated subqueries is supported since Hive 0.13 which was released more than 3 years ago, on 21 April, 2014.

select * from A where id not in (select id from B where id is not null);
+----+--------+
| id |  name  |
+----+--------+
|  3 | George |
+----+--------+
On earlier versions the column of the outer table should be qualified with the table name/alias.

hive> select * from A where id not in (select id from B where id is not null);
FAILED: SemanticException [Error 10249]: Line 1:22 Unsupported SubQuery Expression 'id': Correlating expression cannot contain unqualified column references.
hive> select * from A where A.id not in (select id from B where id is not null);
OK
3   George

P.s.
When using NOT IN you should add is not null to the inner query, unless you are 100% sure that the relevant column does not contain null values.
**One null value is enough to cause your query to return no results.**

- the SQL standard treats x not in (a,b,c) as x<>a and x<>b and x<>c. If for example c is NULL then x<>c is UNKNOWN, therefore the whole expression is UNKNOWN. UNKNOWN is treated as FALSE. That means that the query is not going to return any rows no matter what x value is.
- ref: https://stackoverflow.com/questions/44714625/how-to-use-not-in-in-hive

## Ref:

- [Not Exist](https://www.tutorialgateway.org/sql-not-exists-operator/)
