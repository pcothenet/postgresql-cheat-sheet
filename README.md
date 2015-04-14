# postgresql-cheat-sheet

## Running queries
`SELECT * FROM pg_stat_activity;`

## Cancel query
`SELECT pg_cancel_backend(pid);`

## pg_settings
```sql
SELECT name, setting, boot_val, reset_val, unit
FROM pg_settings
ORDER BY name;
```

## Vacuum
`VACUUM table;`
`ANALYZE VACUUM table;`
`VACUUM FULL table;`

## Last autovacuum
```SQL
SELECT last_autovacuum, last_autoanalyze
FROM pg_stat_user_tables;
```
