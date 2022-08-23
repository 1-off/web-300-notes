
## chr
chr the quote simbol won't work with functions like copy. Example below.
```sql
select chr(119) || chr(48) || chr(48) || chr(116) ;
```
## notmal quotes
```sql
select 'w00t';
```
## double dollar
```sql
select $$w00t$$;
copy * to $$\<path>\output.txt$$;
```
## tag 
```sql
select $TAG$w00t$TAG$;
```
