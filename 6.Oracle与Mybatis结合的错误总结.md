Oracle ：错误总结

1. ORA-00984: 列在此处不允许 :

错误原因： 把“ ”  双引号 改成  ' ' 单引号

> [2020-09-01 16:19:08] [42000][984] ORA-00984: 列在此处不允许
> [2020-09-01 16:19:08] Position: 716
> PVSS> insert into PM_PROJECT (ID, PROJECT_NAME, PARENT_ID, PROJECT_NUMBER, PROJECT_ABBREVIATION, PROJECT_TYPE,
>               THIRD_PROJECTID, CREATE_TIME, PLAN_STARTDATE, PLAN_ENDDATE, REAL_STARTDATE, REAL_ENDDATE, PROJECT_STATE,
>               CREATE_BY, CATEGORY_NAME, CONTRACT_AMOUNT)
>         values ("1", "压力容器PVSS", "1", "1", "PPP", "数字化交付", "1",
>                    to_timestamp('2019-7-19 23:23:23','yyyy-mm-dd hh24:mi:ss'),
>                     to_timestamp('2019-7-19 23:23:23','yyyy-mm-dd hh24:mi:ss'),
>                     to_timestamp('2019-7-19 23:23:23','yyyy-mm-dd hh24:mi:ss'),
>                     to_timestamp('2019-7-19 23:23:23','yyyy-mm-dd hh24:mi:ss'),
>                     to_timestamp('2019-7-19 23:23:23','yyyy-mm-dd hh24:mi:ss'),
>                    "0","hrx","pvss","100")



2. NVARCHAR2 数据库字段映射到mybatis xml， 实体类是String类型

   如果该字段为空“null", 就会报错。

```sql
"DEVICE_NUMBER" NVARCHAR2(64) NULL 
```

```xml
<result column="DEVICE_NUMBER" jdbcType="OTHER" property="deviceNumber" />
```



>Cause: org.apache.ibatis.type.TypeException: Error setting null for parameter #5 with JdbcType OTHER . Try setting a different JdbcType for this parameter or a different jdbcTypeForNull configuration property. Cause: java.sql.SQLException: 无效的列类型: 1111





3. Number 类型的字段长度超了（数据库设置长度为1，测试数据为两位数）

 Cause: java.sql.SQLDataException: ORA-01438: 值大于为此列指定的允许精度



4. xml 语句写的有问题

 Cause: java.sql.SQLSyntaxErrorException: ORA-00907: 缺失右括号



5. xml 中的SQL语句写错了， values 内容写错了

 Cause: java.sql.SQLSyntaxErrorException: ORA-00917: 缺失逗号



6.  数据库中not null 字段，没有插入值， 程序自动设置为空。

   Cause: java.sql.SQLIntegrityConstraintViolationException: ORA-01400: 无法将 NULL 插入 ("PVSS"."PM_WELDING_PROCESS"."ID")



7.  mybatis yml 下的配置：注意打印日志是找错的关键

```yml
#mybatis配置
mybatis:
  mapper-locations: classpath*:mapper/*.xml
  configuration:
    cache-enabled: false # 关闭Mybatis二级缓存
    #日志打印
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```



8.resultType="java.util.HashMap"  可以接收List<Map<String, Object>> 类型 

```xml
<select id="getNoRecordWeldUnderEquipOfWeldProcess" resultType="java.util.HashMap" parameterType="java.lang.String">
          select DISTINCT w.id, w.WELD_TAGNUMBER from PM_WELD w
          inner join PM_EQUIPMENT equ on w.EQUIPMENT_ID = equ.ID
          left join PM_WELDING_PROCESS wp on w.id = wp.weld_id
          where wp.id is null
          AND equ.id = #{equipmentId}
</select>
```



9.  List<>  结果用resultMap="BaseResultMap"接收

   ```XML
   <select id="selectPmWeldingProcessByEquId" resultMap="BaseResultMap" parameterType="java.lang.String">
   
     SELECT * from PM_WELDING_PROCESS
   
     where EQUIPMENT_ID = #{equipmentId}
   
     order by EQUIPMENT_ID ASC
   </select>
   ```

10. Mybatis 数据库使用PageHelper分页

    ```yml
    # yml中分页配置
    pagehelper:
      helper-dialect: oracle
      reasonable: true
      support-methods-arguments: true
    ```

    ```xml
    <!-- pom.xml引入依赖-->
    <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper</artifactId>
        <version>5.1.10</version>
    </dependency>
    ```

代码调用

```java
// 1. 
PageHelper.startPage(Integer.parseInt(pageNum), Integer.parseInt(pageSize));

// 2.
final PageInfo<PmWeldingProcess> pageInfo = new PageInfo<>(processServiceList);

// 3. 
data.put("total", pageInfo.getTotal());
data.put("rows", pageInfo.getList());
```
