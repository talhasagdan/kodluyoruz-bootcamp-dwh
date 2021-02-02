## Soru 3)


```SQL

with tidy as
  (
  select time_trunc(view_time,minute) as t,APPROX_COUNT_DISTINCT(deviceid)  as approx_c
from (
   select 
      extract (time from view_ts) as view_time,
      deviceid
   from talha_sagdan.pageview)
   group by time_trunc(view_time,minute))
   
select
        t,
        approx_c 
        + (case when lag(approx_c,1) over(order by t) is null then 0 else lag(approx_c,1) over(order by t) end )
        + (case when lag(approx_c,2) over(order by t) is null then 0 else lag(approx_c,2) over(order by t) end )
        + (case when lag(approx_c,3) over(order by t) is null then 0 else lag(approx_c,3) over(order by t) end )
        + (case when lag(approx_c,4) over(order by t) is null then 0 else lag(approx_c,4) over(order by t) end ) as active_user
from tidy
order by t;

```
