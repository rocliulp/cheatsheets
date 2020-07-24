# with ... as…create a tmp table in memory
``` sql
-- Replace :XXXX with your data
-- Create a tmp table in memory with your data & find records which is in mytable already.
with var_nos(no_id, no_name) as (values (id1,name1),(id2,name2)) select no_id, no_name from var_nos left outer join mytable on mytable.rec_id = var_nos.no_id where mytable.rec_id is null;
insert into mytable (rec_id, rec_name) values (id1,name1),(id2,name2);

```