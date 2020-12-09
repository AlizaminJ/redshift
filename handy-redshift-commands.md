- NOTE: SQL workbench can sometimes be locked in 'hang' status (showing 'executing statement')- Always check redshift 'load' console to see if a table really loaded

- Checking duplicate rows:
```
SELECT username, email, COUNT(*)
FROM users
GROUP BY username, email
HAVING COUNT(*) > 1
```

- Check stl_load_commits table to see the last commit:
```
select query, trim(filename) as file, curtime as updated
from stl_load_commits order by updated desc;
```

- Check data distribution on all tables:
```
select slice, col, num_values, minvalue, maxvalue
from svv_diskusage
where name='customer' and col=0
order by slice,col;
```

- Look fir disk spills:
```
select query, step, rows, workmem, label, is_diskbased
from svl_query_summary
where query = [YOUR-QUERY-ID]
order by workmem desc;
```

- Check column, distkey, sortkey for a given table:
```
select "column", type, encoding, distkey, sortkey, "notnull" 
from pg_table_def
where tablename = 'lineorder';
```

- To list all tables:
```
SELECT * FROM information_schema.tables;
```

- To list tables in public schema:
```
SELECT table_name FROM information_schema.tables WHERE table_schema='public'
```
