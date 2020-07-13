### 1.建库建表
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
### 2.多表联动查询（父表->子表）
```XML
例如：查询巡检路线下的所有巡检项
select 父表  where 条件 in (    select 子表 where 条件 in (            ));
SQL语句：
<select id="selectAllMonitoringDataByRouteId" resultType="java.lang.String">
    select id from MONITORING_DATA where MONITORING_DATA.INSPECTION_ITEM_ID in (
        select id from INSPECTION_ITEM  WHERE INSPECTION_ITEM.SITE_ID in(
          select INSPECTION_SITE.ID FROM INSPECTION_SITE LEFT JOIN INSPECTION_ITEM on INSPECTION_ITEM.SITE_ID = INSPECTION_SITE.ID
            WHERE  1 =1
            <if test="inspectionRouteId != null and inspectionRouteId != ''">
               INSPECTION_ROUTE_ID = #{inspectionRouteId}
             </if>
   )
)
  </select>
```

### 3.多表联动查询（子表->父表）
```XML
例如：查询某一巡检项所属的巡检路线
子表 left join 父表 on where id1 = id2
```

### 3.多表联动插入
```XML
1. 先插入父表
2. 再插入子表
```

### 4.多表联动更新
```XML
注意：注意外键关联的字段和主键id要区分开（不能用同一个）
1. 先更新子表
2. 再更新父表
```

### 5.多表联动删除
```XML
1. 查子表 ，删子表 如果不是子表会报视图错误
2. 联动删除时注意 删除条件不能是外键，建议用id
3. 删除父表 最后删除父表

<!-- 1. 必须查出来的是子表 2. 删除条件必须是外键 否则会报错 -->
<delete id="deleteMonitorPointLimits" parameterType="java.lang.Integer">
    DELETE FROM (
        SELECT m.* FROM TAG_NUM__MONITOR_POINT t,MONITOR_POINT_LIMITS m
        WHERE t.ID = m.ID
        AND m.ID = #{id, jdbcType=DECIMAL}
    )
</delete>

<!-- 2. 删除位号和测点表同时联动删除测点上下限表数据-->
<delete id="deleteTagNumMonitorPoint" parameterType="java.lang.Integer">
      DELETE FROM TAG_NUM__MONITOR_POINT where TAG_NUM__MONITOR_POINT.ID = #{id, jdbcType=DECIMAL}
</delete>

```

### 6.删除数据库的序列sequence
```XML
删除flyway 的数据表
1. 先进入tag_num__monitor_point表，再删除sequence 
2. drop sequence tag_num__monitor_point_seq;
3. 删除所有MengDa表，否则flyway报错。
4. 启动项目重新生成项目下的所有表
```
### 8.  oracle 数据库获取主键自增长的ID
```XML 标签 加入如下字段
keyProperty="id" keyColumn="ID" useGeneratedKeys="true"
<insert id="insertInTagNumMonitorPoint" keyProperty="id" keyColumn="ID" useGeneratedKeys="true" parameterType="com.fulongtech.rtds.entity.TagNumMonitorPoint">
    INSERT INTO TAG_NUM__MONITOR_POINT (MONITOR_POINT, K_DESCRIPTOR, DEVICE, TAG_NUM, UNIT, CREATEDATE, ID)
    VALUES (#{monitorPoint,jdbcType=VARCHAR}, #{kDescriptor,jdbcType=VARCHAR}, #{device,jdbcType=VARCHAR},
            #{tagNum,jdbcType=VARCHAR}, #{unit,jdbcType=VARCHAR}, #{createDate,jdbcType=VARCHAR}, #{id,jdbcType=VARCHAR})
</insert>


要在代码中
1. 先获取父表的自增长ID ，取出来
2. 插入到子表ID
3. 原因是ID是子表和父表的唯一外键
```
```java
/**
 * 新增一条测点位号
 * @param tagNumMonitorPoint
 * @return
 */
@Override
@Transactional(rollbackFor=Exception.class)
public Result insertTagNumMonitorPoint(final TagNumMonitorPoint tagNumMonitorPoint) {

    try {

        tagNumMonitorPoint.setCreateDate(systemCurrentTime);
        // 1. 先在父表中插入
        tagNumMonitorPointMapper.insertInTagNumMonitorPoint(tagNumMonitorPoint);

        // 2. 获取父表的自增长id
        BigDecimal id = tagNumMonitorPoint.getId();

        // 3. 注入到子表的id
        tagNumMonitorPoint.setId(id);

        // 4. 最后插入子表中
        limitsMapper.insertInMonitorPointLimits(tagNumMonitorPoint);

        return ResultUtil.success("插入数据成功成功。");
    } catch (Exception e) {
        e.printStackTrace();
        return ResultUtil.error(102, "插入数据失败。");
    }
}
```

