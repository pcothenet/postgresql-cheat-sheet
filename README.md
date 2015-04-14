# postgresql-cheat-sheet

## Running queries
`SELECT * FROM pg_stat_activity;`

## Cancel query
`SELECT pg_cancel_backend(pid);`

## pg_settings
```sql
SELECT 
  name, setting, boot_val, reset_val, unit
FROM pg_settings
ORDER BY name;
```

## Show size of biggest tables
```SQL
SELECT 
  nspname || '.' || relname AS "relation",
  pg_size_pretty(pg_total_relation_size(C.oid)) AS "total_size"
FROM pg_class C
LEFT JOIN pg_namespace N ON (N.oid = C.relnamespace)
WHERE nspname NOT IN ('pg_catalog', 'information_schema')
  AND C.relkind <> 'i'
  AND nspname !~ '^pg_toast'
ORDER BY pg_total_relation_size(C.oid) DESC
LIMIT 20;
```

## Show size of biggest relations
```SQL
SELECT 
  nspname || '.' || relname AS "relation",
  pg_size_pretty(pg_relation_size(C.oid)) AS "size"
FROM pg_class C
LEFT JOIN pg_namespace N ON (N.oid = C.relnamespace)
WHERE nspname NOT IN ('pg_catalog', 'information_schema')
ORDER BY pg_relation_size(C.oid) DESC
LIMIT 20;
```



## Vacuum
`VACUUM table;`
`ANALYZE VACUUM table;`
`VACUUM FULL table;`

## Last autovacuum
```SQL
SELECT 
  last_autovacuum, last_autoanalyze
FROM pg_stat_user_tables;
```
