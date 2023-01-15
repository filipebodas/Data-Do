# **OVER CLAUSE**

The `OVER ()` clause allows you to specify the set of rows within a query that the aggregate function should operate on, rather than operating on the entire result set.

In this example, the `OVER ()` clause is not making use of the `PARTITION BY`. This means that the entire dataset from the query is one and only partition.

Consider this sample[^note].

|rating|
|---|
|PG-13|
|R|
|NC-17|
|PG-13NC-17|
|R|
|R|
|R|
|PG-13|
|R|
|PG|

The goal is to find out how many times each rating appears and its respective percentage. To get the denominator to calculate the percentage we apply the `OVER ()` clause and have the entire table as one partition.

```sql
SELECT
  rating,
  COUNT(*) as frequency,
  ROUND(
    COUNT(*) :: NUMERIC / SUM (COUNT(*)) OVER (),
    2
  ) as percentage
FROM
  sample
GROUP BY
  rating
ORDER BY
  frequency desc;
```

Rated-R is the most frequent rating from our sample data appearing in 35% of the total observations.

rating | frequency | percentage
--- | --- | ---
R | 7 | 0.35
PG-13 | 6 | 0.30
NC-17 | 3 | 0.15
G | 2 | 0.10
PG | 2 | 0.10

You can also use other window functions like SUM(), AVG(), COUNT(), MIN(), MAX() and many more with OVER() statement.

[^note]: Taken from Data Wit Danny's [Serious SQL Course](https://www.datawithdanny.com/)

