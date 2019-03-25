---
title: 统计sql
date: 2019-03-25 17:41:01
tags:
---

``` sql
select
round(sum(a.storage)/1024/1024/1024,2) as total,
sales
from a
left join(
  select custom_id,GROUP_CONCAT(distinct sales_name SEPARATOR ',') as sales
  FROM b
  GROUP BY custom_id
) b ON (a.custom_id = b.custom_id)
group by a.id order by total desc limit 50;
```