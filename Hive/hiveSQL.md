## 连接插入表
INSERT OVERWRITE TABLE `58pic_bg_user`
SELECT u.id AS uid,u.create_time AS create_time,u.last_login_time AS last_login_time FROM `58pic_user` AS u
INNER JOIN 
(
   SELECT DISTINCT(hd.uid) FROM `58pic_history_dl` as hd
   JOIN `58pic_pic` as p ON hd.pid = p.id
   WHERE p.kid in (
       SELECT ik.kid FROM `58pic_industry` i
       JOIN `58pic_industry_kid` ik ON i.id = ik.did 
       WHERE i.id in (8,12,13,14,15)
   )
) tempt
ON  u.id = tempt.uid



## CASE WHEN ELSE、substr、collect_set、concat_ws,CASE(colum AS STRING)的常用用法
SELECT id,(
    CASE 
        WHEN substr(vip_arr[0],0,1) = 1 THEN vip_arr[0] 
        WHEN substr(vip_arr[1],0,1) = 1 THEN vip_arr[1] 
        WHEN substr(vip_arr[2],0,1) = 1 THEN vip_arr[2] 
        ELSE NULL
    END
    )
    as vip1,
    (
    CASE 
        WHEN substr(vip_arr[0],0,1) = 2 THEN vip_arr[0] 
        WHEN substr(vip_arr[1],0,1) = 2 THEN vip_arr[1]
        WHEN substr(vip_arr[2],0,1) = 2 THEN vip_arr[2] 
        ELSE NULL
    END
    )
    as vip2,
    (
    CASE 
        WHEN substr(vip_arr[0],0,1) = 3 THEN vip_arr[0] 
        WHEN substr(vip_arr[1],0,1) = 3 THEN vip_arr[1] 
        WHEN substr(vip_arr[2],0,1) = 3 THEN vip_arr[2] 
        ELSE NULL
    END
    )
    as vip3
FROM
(
SELECT id,sort_array(collect_set(concat_ws(',',CAST(type AS STRING),CAST(vip_life AS STRING),CAST(is_limit AS STRING),CAST(expired AS STRING)))) as vip_arr 
FROM `58pic_vip_tmp` GROUP BY id 
) t


###  vip历史记录表
INSERT OVERWRITE TABLE `58pic_vip_record_total`
SELECT
    uid,(
    CASE 
        WHEN split(record1,'-')[0] = 5 AND (split(record1,'-')[3] = '99.00' OR split(record1,'-')[3] = '94.00') THEN '共享不限'
        WHEN split(record1,'-')[0] = 5 AND split(record1,'-')[3] = '98.00' THEN '原创不限'
        WHEN split(record1,'-')[0] = 5 AND split(record1,'-')[3] = '97.00' THEN '办公不限'
        WHEN split(record1,'-')[0] = 5 AND split(record1,'-')[3] = '49.00' THEN '共享20'
        WHEN split(record1,'-')[0] = 5 AND split(record1,'-')[3] = '48.00' THEN '原创20'
        WHEN split(record1,'-')[0] = 5 AND split(record1,'-')[3] = '67.00' THEN '办公10'
        WHEN split(record1,'-')[0] = 5 AND split(record1,'-')[3] = '19.00' THEN '共享20'
        WHEN split(record1,'-')[0] = 5 AND split(record1,'-')[3] = '27.00' THEN '办公10'
        WHEN split(record1,'-')[0] = 5 AND split(record1,'-')[3] = '199.00' THEN '共享不限/原创不限'
        WHEN split(record1,'-')[0] != 5 THEN '活动VIP'
        ELSE 'NULL'
    END
    ) as type1,
    from_unixtime(CAST(split(record1,'-')[1] AS BIGINT),'yyyy-MM-dd HH:mm:ss') as type1_start,
    from_unixtime(CAST(split(record1,'-')[2] AS BIGINT),'yyyy-MM-dd HH:mm:ss') as type1_end,
    (
    CASE 
        WHEN split(record1,'-')[0] = 5  THEN '充值获取VIP'
        WHEN split(record1,'-')[0] != 5 THEN '活动获取VIP'
        ELSE 'NULL'
    END
    ) as type1_reason,
    (
    CASE
        WHEN split(record2,'-')[0] = 5 AND (split(record2,'-')[3] = '99.00' OR split(record2,'-')[3] = '94.00') THEN '共享不限'
        WHEN split(record2,'-')[0] = 5 AND split(record2,'-')[3] = '98.00' THEN '原创不限'
        WHEN split(record2,'-')[0] = 5 AND split(record2,'-')[3] = '97.00' THEN '办公不限'
        WHEN split(record2,'-')[0] = 5 AND split(record2,'-')[3] = '49.00' THEN '共享20'
        WHEN split(record2,'-')[0] = 5 AND split(record2,'-')[3] = '48.00' THEN '原创20'
        WHEN split(record2,'-')[0] = 5 AND split(record2,'-')[3] = '67.00' THEN '办公10'
        WHEN split(record2,'-')[0] = 5 AND split(record2,'-')[3] = '19.00' THEN '共享20'
        WHEN split(record2,'-')[0] = 5 AND split(record2,'-')[3] = '27.00' THEN '办公10'
        WHEN split(record2,'-')[0] = 5 AND split(record2,'-')[3] = '199.00' THEN '共享不限/原创不限'
        WHEN split(record2,'-')[0] != 5 THEN '活动VIP'
        ELSE 'NULL'
    END
    ) as type2,
    from_unixtime(CAST(split(record2,'-')[1] AS BIGINT),'yyyy-MM-dd HH:mm:ss') as type2_start,
    from_unixtime(CAST(split(record2,'-')[2] AS BIGINT),'yyyy-MM-dd HH:mm:ss') as typt2_end,
    (
        CASE 
            WHEN split(record2,'-')[0] = 5  THEN '充值获取VIP'
            WHEN split(record2,'-')[0] != 5 THEN '活动获取VIP'
            ELSE 'NULL'
        END
    ) as type2_reason,
    (
        CASE 
            WHEN split(record3,'-')[0] = 5 AND (split(record3,'-')[3] = '99.00' OR split(record3,'-')[3] = '94.00') THEN '共享不限'
            WHEN split(record3,'-')[0] = 5 AND split(record3,'-')[3] = '98.00' THEN '原创不限'
            WHEN split(record3,'-')[0] = 5 AND split(record3,'-')[3] = '97.00' THEN '办公不限'
            WHEN split(record3,'-')[0] = 5 AND split(record3,'-')[3] = '49.00' THEN '共享20'
            WHEN split(record3,'-')[0] = 5 AND split(record3,'-')[3] = '48.00' THEN '原创20'
            WHEN split(record3,'-')[0] = 5 AND split(record3,'-')[3] = '67.00' THEN '办公10'
            WHEN split(record3,'-')[0] = 5 AND split(record3,'-')[3] = '19.00' THEN '共享20'
            WHEN split(record3,'-')[0] = 5 AND split(record3,'-')[3] = '27.00' THEN '办公10'
            WHEN split(record3,'-')[0] = 5 AND split(record3,'-')[3] = '199.00' THEN '共享不限/原创不限'
            WHEN split(record3,'-')[0] != 5 THEN '活动VIP'
            ELSE 'NULL'
        END
    ) as type3,
    from_unixtime(CAST(split(record3,'-')[1] AS BIGINT),'yyyy-MM-dd HH:mm:ss'),
    from_unixtime(CAST(split(record3,'-')[2] AS BIGINT),'yyyy-MM-dd HH:mm:ss'),
    (
        CASE 
            WHEN split(record3,'-')[0] = 5  THEN '充值获取VIP'
            WHEN split(record3,'-')[0] != 5 THEN '活动获取VIP'
            ELSE 'NULL'
        END
    ) as type3_reason
FROM `58pic_vip_record`d


### 历史下载记录表
SELECT uid,
      split(dl_arr[0],',')[6] as title1,
      split(dl_arr[0],',')[1] as pid1,
      split(dl_arr[0],',')[3] as dname1,
      split(dl_arr[0],',')[4] as kname1,
      split(dl_arr[0],',')[5] as bigclassname1,
      split(dl_arr[0],',')[0] as dltime1,
      split(dl_arr[0],',')[2] as updatetime1,
       
      split(dl_arr[1],',')[6] as title2,
      split(dl_arr[1],',')[1] as pid2,
      split(dl_arr[1],',')[3] as dname2,
      split(dl_arr[1],',')[4] as kname2,
      split(dl_arr[1],',')[5] as bigclassname2,
      split(dl_arr[1],',')[0] as dltime2,
      split(dl_arr[1],',')[2] as updatetime2,
       
      split(dl_arr[2],',')[6] as title3,
      split(dl_arr[2],',')[1] as pid3,
      split(dl_arr[2],',')[3] as dname3,
      split(dl_arr[2],',')[4] as kname3,
      split(dl_arr[2],',')[5] as bigclassname3,
      split(dl_arr[2],',')[0] as dltime3,
      split(dl_arr[2],',')[2] as updatetime3,
      
      split(dl_arr[3],',')[6] as title4,
      split(dl_arr[3],',')[1] as pid4,
      split(dl_arr[3],',')[3] as dname4,
      split(dl_arr[3],',')[4] as kname4,
      split(dl_arr[3],',')[5] as bigclassname4,
      split(dl_arr[3],',')[0] as dltime4,
      split(dl_arr[3],',')[2] as updatetime4,
      
      split(dl_arr[4],',')[6] as title5,
      split(dl_arr[4],',')[1] as pid5,
      split(dl_arr[4],',')[3] as dname5,
      split(dl_arr[4],',')[4] as kname5,
      split(dl_arr[4],',')[5] as bigclassname5,
      split(dl_arr[4],',')[0] as dltime5,
      split(dl_arr[4],',')[2] as updatetime5
FROM (
    SELECT dt.uid,sort_array(collect_set(concat_ws(',',CAST(dt.dltime AS STRING),CAST(dt.pid AS STRING),CAST(dt.updatetime AS STRING),CAST(i.dname AS STRING),CAST(k.kname AS STRING),CAST(b.bigclassname AS STRING),CAST(dt.title AS STRING)))) as dl_arr 
    FROM `58pic_dl_t1` dt
    LEFT JOIN `58pic_kid` k ON dt.kid = k.id 
    LEFT JOIN `58pic_bigclass` b ON dt.bigclassid = b.id 
    LEFT JOIN `58pic_industry_kid` ik ON ik.kid = dt.kid
    LEFT JOIN `58pic_industry` i ON i.id = ik.did
    GROUP BY dt.uid
)tmp











CREATE TABLE `58pic_6369_tmp3` AS
SELECT t.id AS uid,collect_set(t.`_c1`)[0] AS create_time,collect_set(t.`_c2`)[0] AS last_login_time,collect_set(t.`_c3`)[0] AS dnum,collect_set(t.`_c4`)[0] as paynum,sort_array(collect_set(p.expired))[size(collect_set(p.expired))-1] as expired FROM `58pic_6369_tmp2` t
LEFT OUTER JOIN `58pic_privilege` p ON t.id = p.uid
GROUP BY t.id


CREATE TABLE `58pic_6369_tmp4` AS
SELECT t3.uid, collect_set(t3.create_time)[0] as create_time,collect_set(t3.last_login_time)[0] as last_login_time,size(collect_set(hd.pid)) as dnum,collect_set(t3.paynum)[0] as paynum,collect_set(t3.expired)[0] as expired,collect_set(hd.pid)[0] as pid FROM `58pic_6369_tmp3` t3
LEFT OUTER JOIN `58pic_history_dl` hd ON t3.uid = hd.uid
GROUP BY t3.uid


CREATE TABLE `58pic_6369_tmp5` AS
SELECT  t4.uid,t4.create_time,t4.last_login_time,t4.dnum,t4.paynum,t4.expired,i.dname FROM `58pic_6369_tmp4` AS t4
LEFT OUTER JOIN `58pic_pic` AS p ON t4.pid = p.id
LEFT OUTER JOIN `58pic_industry_kid` AS ik ON ik.kid = p.kid
LEFT OUTER JOIN `58pic_industry` AS i ON i.id = ik.did

CREATE TABLE `58pic_6369_tmp6` as
SELECT t5.uid,from_unixtime(t5.create_time,'yyyy-MM-dd HH:mm:ss'),from_unixtime(t5.last_login_time,'yyyy-MM-dd HH:mm:ss'),t.c_size,t5.dnum,t5.paynum,from_unixtime(t5.expired,'yyyy-MM-dd HH:mm:ss'),t5.dname FROM `58pic_6369_tmp5` as t5
LEFT OUTER JOIN `58pic_6369_tmp` as t ON t.id = t5.uid


CREATE TABLE `58pic_6369_tmp1` AS
SELECT tp.id,collect_set(tp.`_c1`)[0] AS create_time,collect_set(tp.`_c2`)[0] as last_login_time,collect_set(tp.c_size)[0] as login_count,size(collect_set(p.pid)) as dnum FROM `58pic_6369_tmp` tp
LEFT OUTER JOIN `58pic_history_dl` p ON tp.id = p.id
GROUP BY tp.id

CREATE TABLE `58pic_6369_tmp3` AS
SELECT t1.id,collect_set(t1.create_time)[0] as create_time,collect_set(t1.last_login_time)[0] as last_login_time,collect_set(t1.login_count)[0] as login_count,collect_set(t1.dnum)[0],
collect_set(
CASE 
    WHEN so.status = 1 THEN so.amount
    ELSE 0
END
) as amount
FROM `58pic_6369_tmp1` t1
LEFT OUTER JOIN `58pic_sponsor_order` so ON t1.id = so.uid
GROUP BY t1.id

SELECT * FROM `58pic_6369_tmp3` t3 
LEFT OUTER JOIN `58pic_privilege` p ON t3.id = p.uid
GROUP BY 

CREATE TABLE `58pic_6369_tmp4` AS 
SELECT t3.id,collect_set(t3.create_time)[0] as create_time,collect_set(t3.last_login_time)[0] as last_login_time,collect_set(t3.login_count)[0] as login_count,collect_set(t3.`_c4`)[0] as dnum,collect_set(t3.amount)[0] as amount,sort_array(collect_set(p.expired))[size(collect_set(p.expired))-1] as expired
FROM `58pic_6369_tmp3` t3
LEFT OUTER JOIN `58pic_privilege` as p ON t3.id = p.uid
GROUP BY t3.id


SELECT 
tp4.id,
collect_set(tp4.create_time)[0],
collect_set(tp4.last_login_time)[0],
collect_set(tp4.login_count)[0],
collect_set(tp4.dnum)[0],
collect_set(tp4.amount)[0],
collect_set(tp4.expired)[0],
collect_set(dl.pid)[0] 
FROM `58pic_6369_tmp4` tp4
LEFT OUTER JOIN `58pic_history_dl` AS dl ON tp4.id = dl.uid
GROUP BY tp4.id



LEFT OUTER JOIN `58pic_pic` AS p ON t4.pid = p.id
LEFT OUTER JOIN `58pic_industry_kid` AS ik ON ik.kid = p.kid
LEFT OUTER JOIN `58pic_industry` AS i ON i.id = ik.did




drop table `58pic_6369_tmp1`


CREATE TABLE `58pic_6369_tmp2` AS
SELECT tp1.id,collect_set(tp1.`_c1`)[0],collect_set(tp1.`_c2`)[0],collect_set(tp1.`_c3`)[0],collect_set(so.amount) FROM `58pic_6369_tmp1` tp1
LEFT OUTER JOIN `58pic_sponsor_order` so ON tp1.id = so.uid
WHERE so.status = '1'
GROUP BY tp1.id







