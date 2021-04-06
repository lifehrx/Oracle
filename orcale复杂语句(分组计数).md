```SQL
select pro.id "id",PRO.project_name "projectName",--项目名称
                PRO.project_number "projectNumber",--项目号
                count(w.id)  "totalNum",--项目焊口总数量（道）
                sum(w.INCHES_DIAMETER)  "diameterNum",--项目总寸经
                sum(case when "INSTR"(w.MATERIAL, '12Cr1MoVG') > 0 then 1 else 0 end)  "oneNum", --12Cr1MoVG 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, '12Cr1MoVG') > 0 then 1 else 0 end)  "oneFinishedNum", --12Cr1MoVG 材质完成数
                sum(case when "INSTR"(w.MATERIAL, '15CrMoG') > 0 then 1 else 0 end)  "twoNum", --15CrMoG 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, '15CrMoG') > 0 then 1 else 0 end)  "twoFinishedNum", --15CrMoG 材质完成数
                sum(case when "INSTR"(w.MATERIAL, 'P22') > 0 then 1 else 0 end)  "threeNum", --P22 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, 'P22') > 0 then 1 else 0 end)  "threeFinishedNum", --P22 材质完成数
                sum(case when "INSTR"(w.MATERIAL, 'P11') > 0 then 1 else 0 end)  "fourNum", --P11 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, 'P11') > 0 then 1 else 0 end)  "fourFinishedNum", --P11 材质完成数
                sum(case when "INSTR"(w.MATERIAL, 'P91') > 0 then 1 else 0 end)  "fiveNum", --P91 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, 'P91') > 0 then 1 else 0 end)  "fiveFinishedNum", --P91 材质完成数
                sum(case when "INSTR"(w.MATERIAL, '12Cr5Mo') > 0 then 1 else 0 end)  "sixNum", --12Cr5Mo 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, '12Cr5Mo') > 0 then 1 else 0 end)  "sixFinishedNum", --12Cr5Mo 材质完成数
                sum(case when "INSTR"(w.MATERIAL, '347') > 0 then 1 else 0 end)  "sevenNum", --347 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, '347') > 0 then 1 else 0 end) "sevenFinishedNum", --347 材质完成数
                sum(case when "INSTR"(w.MATERIAL, 'F53') > 0 then 1 else 0 end) "eightNum", --F53 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, 'F53') > 0 then 1 else 0 end) "eightFinishedNum", --F53 材质完成数
                sum(case when "INSTR"(w.MATERIAL, 'S31803') > 0 then 1 else 0 end) "nineNum", --S31803 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, 'S31803') > 0 then 1 else 0 end) "nineFinishedNum", --S31803 材质完成数
                sum(case when "INSTR"(w.MATERIAL, 'N06600') > 0 then 1 else 0 end) "tenNum", --N06600 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, 'N06600') > 0 then 1 else 0 end) "tenFinishedNum", --N06600 材质完成数
                sum(case when "INSTR"(w.MATERIAL, 'N08825') > 0 then 1 else 0 end) "elevenNum", --N08825 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, 'N08825') > 0 then 1 else 0 end) "elevenFinishedNum", --N08825 材质完成数
                sum(case when "INSTR"(w.MATERIAL, '316') > 0 then 1 else 0 end) "twlveNum", --316 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, '316') > 0 then 1 else 0 end) "twlveFinishedNum", --316 材质完成数
                sum(case when "INSTR"(w.MATERIAL, '321') > 0 then 1 else 0 end) "firstNum", --321 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, '321') > 0 then 1 else 0 end) "firstFinishedFum", --321 材质完成数
                sum(case when "INSTR"(w.MATERIAL, '304') > 0 then 1 else 0 end) "secondNum", --304 材质总数
                sum(case when wp.id is not null and "INSTR"(w.MATERIAL, '304') > 0 then 1 else 0 end) "secondFinishedNum", --304 材质完成数
                '' "remark" --备注
            from tb_cm_weld w
            left join tb_cm_pipeline p on w.pipeline_id = p.id and p.is_available = '1'
            left join TB_CM_WELDING_PROCESS wp on w.id = wp.weld_id and wp.repair_identify = 0 and wp.is_available = '1'
            left join tb_sys_project pro on p.project_id = PRO."ID"
            where w.is_available='1' and PRO."ID" is not null and ("INSTR"(w.MATERIAL, '12Cr1MoVG') > 0 or "INSTR"(w.MATERIAL, '15CrMoG') > 0
            or "INSTR"(w.MATERIAL, 'P22') > 0 or "INSTR"(w.MATERIAL, 'P11') > 0 or "INSTR"(w.MATERIAL, 'P91') > 0
            or "INSTR"(w.MATERIAL, '12Cr5Mo') > 0 or "INSTR"(w.MATERIAL, '347') > 0 or "INSTR"(w.MATERIAL, 'F53') > 0
            or "INSTR"(w.MATERIAL, 'S31803') > 0 or "INSTR"(w.MATERIAL, 'N06600') > 0 or "INSTR"(w.MATERIAL, 'N08825') > 0
            or "INSTR"(w.MATERIAL, '316') > 0 or "INSTR"(w.MATERIAL, '321') > 0 or "INSTR"(w.MATERIAL, '304') > 0)
            group by pro.id,PRO.project_name,PRO.project_number;
```            
