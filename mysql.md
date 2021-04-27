# with ... as…create a tmp table in memory
``` sql
-- Replace :XXXX with your data
-- Create a tmp table in memory with your data & find records which is in mytable already.
with var_nos(no_id, no_name) as (values (id1,name1),(id2,name2)) select no_id, no_name from var_nos left outer join mytable on mytable.rec_id = var_nos.no_id where mytable.rec_id is null;
insert into mytable (rec_id, rec_name) values (id1,name1),(id2,name2);
```

```bash
# mysqldump for backup
# The -d flag says not to include data in the dump. Alternatively you can use –no-data instead if you find that easier to remember:

# Dumping the database structure for all tables with no data
mysqldump -d -u someuser -p mydatabase
mysqldump --no-data -u someuser -p mydatabase
mysqldump --no-data -u someuser -papples mydatabase
# Dumping the database structure for one table with no data
mysqldump -d -u someuser -p mydatabase products
# Dumping the database structure for several table with no data
mysqldump -d -u someuser -p mydatabase products categories users
# Dumping the structure to a file
mysqldump -d -u someuser -p mydatabase > mydatabase.sql
mysql -u someuser -p anotherdatabase < mydatabase.sql
```

