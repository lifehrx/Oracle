### 1.建库建表
创建表空间步骤
最高权限用户：
用户名：MengDa
密  码：root

1.sqlplus /nolog

2.connect sys/orcl@ORCL as sysdba

3.查看表空间的语句：
    Select * From Dba_Tablespaces；

1.创建表空间
create tablespace MengDa
    logging
    datafile 'F:\app\pc\oradata\MengDa.dbf'
    size 32m
    autoextend on
    maxsize unlimited
    extent management local;
    
2.创建用户
create user MengDa identified by root
    default tablespace MengDa                            
    temporary tablespace temp;
    
3.改用户赋权限
grant connect to MengDa with admin option;

grant resource to MengDa with admin option;

grant aq_administrator_role to MengDa with admin option;

grant authenticateduser to MengDa with admin option;

grant dba to MengDa with admin option;

grant unlimited tablespace to MengDa with admin option;

### 2.多表联动查询

### 3.多表联动插入

### 4.多表联动更新

### 5.多表联动删除

### 6.删除数据库的序列sequence
