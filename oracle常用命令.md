### 1.【cmd 下执行切换至SQL】
> sqlplus /nolog 

### 2.【cmd 下执行切换至SQL】
> connect sys/orcl@ORCL as sysdba 【连接数据库】

### 3.【查看数据库版本号】
> select * from v$version; 

### 4.【导入导出的dmp文件】exp 和 expd
> expdp system(用户名)/system(用户密码)@orcl directory=dmp schemas=air20170724(数据库名) dumpfile=air20170724.dmp(导出的文件名)
>
> impdp system(用户名)/system(用户密码)@orcl schemas=air(数据库名) directory=dmp dumpfile=air20170724.dmp
>
> 注意：数据库导入导出的版本是否一致否则要加版本号
>
> expdp system/root@orcl directory=dmp schemas=air_mengda dumpfile=air_mengda.dmp version=11.1.0.6.0
>
> impdp system/root@orcl schemas=air_mengda directory=dmp dumpfile=AIR_MENGDA.dmp  remap_schema=air:air_mengda remap_tablespace=air:air_mengda 
>
> exp air_ZhongHeZongTu_220ZhongHeZongTu/root@192.168.10.122:1521/air_ZhongHeZongTu_220 file=F:\项目部署资料\中核总图DEMO\ZhongHeZongTu.dmp

### 5.删除表空间
> sqlplus /nolog
>
> connect sys/orcl@ORCL as sysdba
>
> drop user 【数据库名】 cascade; 
>
> drop tablespace 【数据库名】 including contents and datafiles;

