select  t0.FID                as 单据内码
       ,t1.FENTRYID           as 分录内码
       ,t0.FBILLNO            as 单据编号
       ,t0.FDate              as 采购日期
       ,t0.FDOCUMENTSTATUS    as 单据状态
       ,t2.FLOCALCURRID       as 本位币
       ,ISNULL(t20.FPRICEDIGITS,4) AS FPRICEDIGITS
       ,ISNULL(t20.FAMOUNTDIGITS,2) AS FAMOUNTDIGITS
       ,t1.FMATERIALID        as 物料
       ,t1M_L.FNAME          as FMaterialName
       ,t1.FQTY                as 数量
       ,t1u.FPRECISION          as FUnitPrecision
       ,t1U_L.FNAME           as FUnitName
       ,t1f.FTAXPRICE
       ,t1f.FALLAMOUNT
       ,row_number() over (order by t0.FBillNo desc) as FIDENTITYID1
  
from T_PUR_POORDER t0
inner join T_PUR_POORDERFIN t2 on (t0.FID = t2.FID)
left join T_BD_CURRENCY t20 on (t2.FLOCALCURRID = t20.FCURRENCYID)
inner join T_PUR_POORDERENTRY t1 on (t0.FID = t1.FID)
left join T_BD_MATERIAL_L t1M_L on (t1.FMATERIALID = t1m_l.FMATERIALID and t1M_L.FLOCALEID = 2052)
inner join T_PUR_POORDERENTRY_F t1F on (t1.FENTRYID = t1f.FENTRYID)
left join T_BD_UNIT t1U on (t1f.FPRICEUNITID = t1u.FUNITID)
left join T_BD_UNIT_L t1U_L on (t1U.FUNITID = t1U_L.FUNITID and t1U_L.FLOCALEID = 2052)
where t0.FBILLNO like '%#FBillNO#%' and t0.FCREATORID = _CurrentUserId_
