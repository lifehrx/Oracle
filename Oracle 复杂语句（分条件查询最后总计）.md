```SQL
-- rt委托已发出焊口
with
entrust_rt_info as (
	select ORD.device_id,NDT.weld_id 
	from tb_cm_ndt_entrust_order ord
	left join tb_cm_nondestructive_test ndt on  ndt.rt_order_id = ORD.id
	where ord.is_available = '1' and ORD.ENTRUST_STATE = 1 and NDT.is_available = '1'
),
-- ut委托已发出焊口
entrust_ut_info as (
	select ORD.device_id,NDT.weld_id 
	from tb_cm_ndt_entrust_order ord
	left join tb_cm_nondestructive_test ndt on  ndt.ut_order_id = ORD.id
	where ord.is_available = '1' and ORD.ENTRUST_STATE = 1 and NDT.is_available = '1'
)
,
-- mt委托已发出焊口
entrust_mt_info as (
	select ORD.device_id,NDT.weld_id 
	from tb_cm_ndt_entrust_order ord
	left join tb_cm_nondestructive_test ndt on  ndt.mt_order_id = ORD.id
	where ord.is_available = '1' and ORD.ENTRUST_STATE = 1 and NDT.is_available = '1'
)
,
-- pt委托已发出焊口
entrust_pt_info as (
	select ORD.device_id,NDT.weld_id 
	from tb_cm_ndt_entrust_order ord
	left join tb_cm_nondestructive_test ndt on  ndt.pt_order_id = ORD.id
	where ord.is_available = '1' and ORD.ENTRUST_STATE = 1 and NDT.is_available = '1'
)
,
-- paut委托已发出焊口
entrust_paut_info as (
	select ORD.device_id,NDT.weld_id 
	from tb_cm_ndt_entrust_order ord
	left join tb_cm_nondestructive_test ndt on  ndt.paut_order_id = ORD.id
	where ord.is_available = '1' and ORD.ENTRUST_STATE = 1 and NDT.is_available = '1'
)
,
-- tofd委托已发出焊口
entrust_tofd_info as (
	select ORD.device_id,NDT.weld_id 
	from tb_cm_ndt_entrust_order ord
	left join tb_cm_nondestructive_test ndt on  ndt.tofd_order_id = ORD.id
	where ord.is_available = '1' and ORD.ENTRUST_STATE = 1 and NDT.is_available = '1'
),
-- 多检测方式合并后的委托已发出的焊口
total_entrust_info as (
	select * from entrust_rt_info 
	union all select * from entrust_ut_info  
	union all select * from entrust_mt_info  
  union all select * from entrust_pt_info  
  union all select * from entrust_paut_info 
  union all select * from entrust_tofd_info 
)
-- 最终多检测方式去重复后的委托已发出焊口数量
SELECT device_id,count(distinct weld_id) entrust_num --委托已发出焊口数量
from total_entrust_info group by device_id
```
