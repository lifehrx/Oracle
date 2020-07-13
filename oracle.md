### 1.建库建表
创建表空间步骤
最高权限用户：
用户名：Test
密  码：root
```
1.sqlplus /nolog

2.connect sys/orcl@ORCL as sysdba

3.查看表空间的语句：
    Select * From Dba_Tablespaces；

1.创建表空间
create tablespace Test
    logging
    datafile 'F:\app\pc\oradata\Test.dbf'
    size 32m
    autoextend on
    maxsize unlimited
    extent management local;
    
2.创建用户
create user Test identified by root
    default tablespace Test                            
    temporary tablespace temp;
    
3.改用户赋权限
grant connect to Test with admin option;

grant resource to Test with admin option;

grant aq_administrator_role to Test with admin option;

grant authenticateduser to Test with admin option;

grant dba to Test with admin option;

grant unlimited tablespace to Test with admin option;
```
### 2.多表联动查询
```
例如：查询巡检路线下的所有巡检项
select 父表  where 条件 in (    select 子表 where 条件 in (            ));
```

### 3.多表联动插入

### 4.多表联动更新

### 5.多表联动删除

### 6.删除数据库的序列sequence
