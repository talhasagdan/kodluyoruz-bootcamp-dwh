## Soru 1) 1980’den itibaren spor grubu bazında en çok madalya alan 1. 3. 5. ülkeyi bulalım.

```SQL
with country_medals as
  (
    select -- last query for filtering 1th,3th,5th countries based row numbers
    sport,
    country as first_goat,
    lead(country,2) over(partition by sport order by r_n) as thirth_goat,
    lead(country,4) over(partition by sport order by r_n) as fifth_goat,
  from
  (select -- second subquery for putting row number by groups
    row_number() over(partition by sport order by c_medals desc) as r_n,
    sport,country,c_medals
   from
   (select -- first subquery for filtering 1980 or higher and counting medals with grouping
      country,
      sport,
      count(country) as c_medals,
    from `dsmbootcamp.talha_sagdan.summer_medals`
    where year >= 1980
    group by country,sport
    order by sport asc , c_medals desc
    )
  )
  where r_n between 1 and 5
  )
  select * -- I just try to projection with logic some fifth counrty column for other results always get null when use lead function
  from country_medals
  where fifth_goat is not null
  ;

```
