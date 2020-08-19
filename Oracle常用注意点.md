> 1. id 字段使用随机数生成，字段长度设置为36.
> ###
> 2. left outer join on = left join on (只是缩写的关系)
> 3. A表 left join on B表 （与A表记录数相同）
> 4. A表 right join on B表 （与B表记录数相同）
> 5. A表 left join on B表 = B表 right join on A表
> 6. 开发中多用 left join on
>  ###
> 7. 索引是关系数据库中对某一列或多个列的值进行预排序的数据结构。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录，这样就大大加快了查询速度。
> ###
> 8. 在设计关系数据表的时候，看上去唯一的列，例如身份证号、邮箱地址等，因为他们具有业务含义，因此不宜作为主键。可以选择UUID做主键。
> ###
> 9. 模糊查询 instr() 比 like 更高效，测试500条数据首次查询
> select * from ITEMS i where i.PARENT_ID like '%f%';     （85ms） 
> select * from ITEMS i where instr(i.PARENT_ID, 'f') > 0;（41ms）
> ###
>
>
