### 1.【cmd 下执行切换至SQL】
```SQL
sqlplus /nolog 
```

### 2.【cmd 下执行切换至SQL】
```SQL
connect sys/orcl@ORCL as sysdba 【连接数据库】
```

### 3.【查看数据库版本号】
```SQL
select * from v$version; 
```

### 4.【导入导出的dmp文件】exp 和 expd
```SQL
impdp system(用户名)/system(用户密码)@orcl schemas=air(数据库名) directory=dmp dumpfile=air20170724.dmp

注意：数据库导入导出的版本是否一致否则要加版本号

expdp system/root@orcl directory=dmp schemas=air_mengda dumpfile=air_mengda.dmp version=11.1.0.6.0

impdp system/root@orcl schemas=air_mengda directory=dmp dumpfile=AIR_MENGDA.dmp  remap_schema=air:air_mengda remap_tablespace=air:air_mengda 

exp air_ZhongHeZongTu_220ZhongHeZongTu/root@192.168.10.122:1521/air_ZhongHeZongTu_220 file=F:\项目部署资料\中核总图DEMO\ZhongHeZongTu.dmp
```

### 5.删除表空间
```SQL
1.sqlplus /nolog
2.connect sys/orcl@ORCL as sysdba
3.drop user 【数据库名】 cascade; 
4.drop tablespace 【数据库名】 including contents and datafiles;
```
### 6.建库建表
创建表空间步骤
最高权限用户：
用户名：Test
密  码：root
```SQL
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

