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

## Get all constraints on a table
```SQL
SELECT tc.constraint_name,
	tc.constraint_type,
	tc.table_name,
	kcu.column_name,
	rc.match_option AS match_type,
	ccu.table_name AS references_table,
	ccu.column_name AS references_field
FROM information_schema.table_constraints tc
LEFT JOIN information_schema.key_column_usage kcu
	ON tc.constraint_catalog = kcu.constraint_catalog
	AND tc.constraint_schema = kcu.constraint_schema
	AND tc.constraint_name = kcu.constraint_name
LEFT JOIN information_schema.referential_constraints rc
	ON tc.constraint_catalog = rc.constraint_catalog
	AND tc.constraint_schema = rc.constraint_schema
	AND tc.constraint_name = rc.constraint_name
LEFT JOIN information_schema.constraint_column_usage ccu
	ON rc.unique_constraint_catalog = ccu.constraint_catalog
	AND rc.unique_constraint_schema = ccu.constraint_schema
	AND rc.unique_constraint_name = ccu.constraint_name
WHERE tc.table_name = 'table_name';
```

## Vacuum
`VACUUM table;`
`ANALYZE VACUUM table;`
`VACUUM FULL table;`

## Last autovacuum
```SQL
SELECT 
  last_autovacuum, last_autoanalyze
FROM pg_stat_user_tables
WHERE last_autovacuum IS NOT NULL
ORDER BY last_autovacuum DESC;
```
