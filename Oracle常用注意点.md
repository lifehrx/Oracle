> 1. id 字段使用随机数生成，字段长度设置为36.
> ###
> 2. left outer join on = left join on (只是缩写的关系)
> 3. A表 left join on B表 （与A表记录数相同）
> 4. A表 right join on B表 （与B表记录数相同）
> 5. A表 left join on B表 = B表 right join on A表
> 6. 开发中多用 left join on
>  ###
> 索引是关系数据库中对某一列或多个列的值进行预排序的数据结构。通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录，这样就大大加快了查询速度。
> ###
>
