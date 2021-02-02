
```diff
- Step 1)
+ car ve manufacture id üzerinden cross join yaparak  ve sonrasındaki selectte array'den struct bir yapıya çevirdik. 
+ car_id, car_model, manufacture_year istediğimiz gibi ulaştık
- Step 2)
+ purchase id ve date array istendiği için array(select as struct... from unnest(..)) yapısını kullandık.
+ 3 saat eklenmesi için timestamp_add(main, target) fonksiyonunda 180 dakika ekleyerek hedefte istenene ulaştık.
```

```SQL
with hw as
  (
    select *
    from talha_sagdan.semi_structured_hw)
  select name,
         car.id as car_id,
         car.model as car_model,
         manufacture.year as manufacture_year,
         array(select as struct temp.id, timestamp_add(temp.date ,interval 180 minute) as date from unnest(purchase)as temp) as purchase

  from hw
  cross join unnest(car) as car
  cross join unnest(manufacture) as manufacture
  on car.id = manufacture.id;
```
