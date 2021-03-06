@testx/keywords-postgres
=====

A library that extends testx with keywords for testing [Postgres](https://www.postgresql.org/) databases.

## How does it work
From the directory of the art code install the package as follows:
```sh
npm install @testx/keywords-postgres --save
```

After installing the package you add these keywords to testx by adding the following line to your protractor config file:

```
testx.keywords.add(require('@testx/keywords-postgres'))
```

## Example **testx** script:
```
- execute sql:
    sql: SELECT 1;
    expected result:
      - ?column?: 1
    save result to: saved
- execute sql:
    sql: SELECT 1;
    expected result: '{{saved}}'
- execute sql:
    sql: |-
      DROP TABLE IF EXISTS test;
      CREATE TABLE test (first varchar(20), second integer);
      INSERT INTO test(first, second) VALUES ('test1', 12), ('test2', 34);
      SELECT * FROM test;
    expected result:
      - first: test1
        second: 12
      - first: test2
        second: 34
```

## Keywords

| Keyword                | Argument name | Argument value  | Description |
| ---------------------- | ------------- | --------------- |------------ |
| execute sql            |               |                 | Connect to the database, execute the SQL query/statement and optionally check the expected result and/or save it in the test context. |
|                        | connection string | Connection string in the format of *postgres://user:password@host:port/database*.| Optional. If not set, the **postgresConnectionString** command line (or config file) parameter will be used.|
|                        | sql             | SQL query/statement to execute. | Required. |
|                        | expected result | Expected result of the query. | Optional. It will be compared to the result of the query. The keyword will fail if they are different. The expected result should be a list of rows. Every row is an object (see the [example](#example)). |
|                        | save result to  | *varname* | Optional. The name of a context variable, that will be used to save the result of the query. |
