# **DUPLICATES**

This one is going to be longer than usual, please bear with me :) 

Going back to the *health.user_logs*[^note]. 

We can make use of the `COUNT(*)` to see how many records exist in our dataset. 

```sql
SELECT COUNT(*)
FROM health.user_logs
```

We found out that we have 43.891 records. Picking up from the last post about [CTE and Temporary Tables](https://github.com/filipebodas/Data-Do/blob/main/SQL/CTE.md) we found out the number of unique rows.

```sql
WITH deduped_logs AS (
  SELECT DISTINCT *
  FROM health.user_logs
)
SELECT COUNT(*)
FROM deduped_logs;
```

We got 31.004 **unique rows** (approximately less 12k rows than the entire dataset). Therefore, proving the existence of duplicated records.


### **GOAL: IDENTIFY THOSE DUPLICATES**

All duplicated
Unique duplicated
Duplicated counts

One strategy to identify all the duplicate is to use a `GROUP BY ` with all the columns and a `COUNT`

The rows that have a count greater than one are the duplicates. In order to reduce the output, we can use the `HAVING` statement to the `COUNT(*)`.

```sql
SELECT
  id,
  log_date,
  measure,
  measure_value,
  systolic,
  diastolic,
  COUNT(*) AS frequency
FROM
  health.user_logs
GROUP BY
  id,
  log_date,
  measure,
  measure_value,
  systolic,
  diastolic
HAVING COUNT(*) > 1
ORDER BY
  frequency DESC
```

[^note]: Taken from Data Wit Danny's [Serious SQL Course](https://www.datawithdanny.com/)