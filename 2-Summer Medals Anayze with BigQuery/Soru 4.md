## Soru 4)

```SQL

merge talha_sagdan.content_category t 
using talha_sagdan.content_category_20201222_00_59 s
      on t.id = s.id
-- eşleşiyor ancak date eşit değil -> kategori değişti
when matched and t.cdc_date!=s.cdc_date then
      update set t.category = s.category,t.cdc_date=s.cdc_date
-- source yeni değer gelmiş -> target'a ekle
when not matched by target then
      insert (cdc_date, is_deleted,id,category)
      values (s.cdc_date,s.is_deleted,s.id,s.category)
-- source'da artık ürün yok -> target silindi olarak güncelle
when not matched by source then
      update set t.is_deleted=true;


```
