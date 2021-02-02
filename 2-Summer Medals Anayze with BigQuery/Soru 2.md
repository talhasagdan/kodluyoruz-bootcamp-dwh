## Soru 2) 1980’den itibaren herhangi bir spor grubunda üst üste 3 veya daha fazla madalya almış atletleri bulalım.

Aşağıdaki şekilde

create or replace table dsmbootcamp.DATASET_ADINIZ.summer_medals

as

select * from dsmbootcamp.sample.summer_medals;

DATASET_ADINIZ kısmına kendi dataset adınızı yazarak tabloyu create ettikten sonra, soru çözümünüzde bu yarattığınız tabloyu kullanabilirsiniz.

```SQL

with athlete_medals as(
      select 
       athlete,
       year,
       ordered_,
       lead(ordered_, 1) over(partition by athlete order by athlete, ordered_) as cons_2,
       lead(ordered_, 2) over(partition by athlete order by athlete, ordered_) as cons_3,
  from (
        select distinct
        cast (((year-1980)/4) +1 as numeric) as ordered_,
        athlete,
        year
        from `dsmbootcamp.talha_sagdan.summer_medals`
        where year>=1980)
        order by athlete,year
  )
  select distinct athlete
  from athlete_medals
  where ordered_+1 = cons_2 and ordered_+2=cons_3
;

```
