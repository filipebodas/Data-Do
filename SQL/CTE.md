# **CTE vs TEMPORARY TABLES**

A CTE (Common Table Expression) is a SQL query that manipulates existing data and stores the data outputs as a new reference.

Lets use one to find unique rows on the table *health.user_logs*[^note]

```sql
WITH deduped_logs AS (
  SELECT DISTINCT *
  FROM health.user_logs
)
SELECT COUNT(*)
FROM deduped_logs;
```

We retrieved all the unique records with the CTE and them we counted how many existed. **Note that this is to identify how many unique records there are, not how many times they repeat themselves**.

The same result can be achieve with a Temporary Table.

```sql
DROP TABLE IF EXISTS deduplicated_user_logs; 

CREATE TEMP TABLE deduplicated_user_logs AS
SELECT DISTINCT *
FROM health.user_logs;

SELECT COUNT(*)
FROM deduplicated_user_logs;
```

 Which one to use? Will I need to use the subset later?
 + **Yes** - Temporary tables
 + **No** - CTEs

[^note]: Taken from Data Wit Danny's [Serious SQL Course](https://www.datawithdanny.com/)