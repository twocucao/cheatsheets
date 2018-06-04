---
title: PostgreSQL
---

Replace anything within `<placeholder>` accordingly

### Console

    $ psql #logs in to default database & default user
    $ sudo -u <rolename:postgres> psql #logs in with a particular user

### Commands

 * Show roles: `\du`
 * Show tables: `\dt`
 * Show databases: `\l`
 * Connect to a database: `\c <database>`
 * Show columns of a table: `\d <table>` or `\d+ <table>`
 * Quit: `\q`

### Creating database

     $ createdb databasename

### MAC 上面开启关闭

    pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log start
    pg_ctl -D /usr/local/var/postgres -l /usr/local/var/postgres/server.log stop
    pg_ctl -D /usr/local/var/postgres start
    pg_ctl -D /usr/local/var/postgres stop -s -m fast

### 强行中断数据库连接

    -- 强行中断连接到此数据库的 session
    SELECT
        pg_terminate_backend(pid)
    FROM
        pg_stat_activity
    WHERE
        -- don't kill my own connection!
        pid <> pg_backend_pid()
        -- don't kill the connections to other databases
        AND datname = 'demoweb' ;

### 数据导入导出

    pg_dump -C -Fp -f dump.sql -U twocucao QCS -h 192.168.2.175
    pg_dump -C -Fp -f 20160602-150144-dump.sql -U twocucao QCS --column-inserts --data-only --table=users_table -h 192.168.2.175
    # 插入数据
    psql -U twocucao -d QCS -a -f insert_doc_ids.sql -h 192.168.2.175
    pg_restore --verbose --clean --no-acl --no-owner -h localhost example.dump

### 3.2 DCL ( Data Control Languge )

    -- 创建只读用户
    \c demoweb
    CREATE ROLE ro_user WITH LOGIN ENCRYPTED PASSWORD 'xxx123456';
    GRANT CONNECT ON DATABASE demoweb TO ro_user;
    -- This assumes you're actually connected to mydb..
    GRANT USAGE ON SCHEMA public TO ro_user;
    GRANT SELECT ON ALL TABLES IN SCHEMA public TO ro_user;
    GRANT SELECT ON ALL SEQUENCES IN SCHEMA public TO ro_user;

    -- 撤销数据库连接 ()
    REVOKE CONNECT ON DATABASE demoweb FROM PUBLIC, demoweb;

### 3.3 DDL ( Data Definition Language )

    CREATE
    ALTER
    DROP
    TRUNCATE
    COMMENT
    RENAME

### 3.4 DML ( Data Manipulation Languge )

    SELECT
    INSERT
    UPDATE
    DELETE
    MERGE
    CALL
    EXPLAIN PLAN
    LOCK TABLE

### 3.5 TCL ( Transaction Control Languge )

    # TCL

### CTE 查询

    WITH cte_name(
        CTE_query_definition -- non-recursive term
        UNION [ALL]
        CTE_query_definition -- recursive term
    ) SELECT * FROM cte_name;

### 备份还原

    # 需要备份的机器
    DB_NAME='xxxdb'
    DUMP_DB_FILE='latest_dump.sql.gz'
    sudo -u postgres pg_dump $DB_NAME | gzip -9 > $DUMP_DB_FILE
    TARGET_HOSTNAME='xxx.org'
    TARGET_PATH='/webapps/'
    scp $DUMP_DB_FILE root@$TARGET_HOSTNAME:/webapps/

    # 需要还原的机器
    DB_NAME='xxxdb'
    DUMP_DB_FILE='latest_dump.sql.gz'
    sudo -u postgres dropdb $DB_NAME
    sudo -u postgres createdb $DB_NAME
    gunzip < $DUMP_DB_FILE | sudo -u postgres psql $DB_NAME

