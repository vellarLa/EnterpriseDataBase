# Лабораторная работа 2

Задачу машинного обучения для БД - предсказать количество (диапазон) штрафов, полученных человеком по основной информации о водителе

Запрос для формирования датасета:

`
select count(id) as "license_categories", min(gender) as "gender", min(experience) as "experience",
min(birthday) as "birthday", sum(count_fines) as "count_fines", min(brand) as "brand", min("number_tr") as "number"
from (
    license l full join
        (select d.id as "driver_id", d.gender as "gender", d.experience as "experience", d.birthday as "birthday", count(f.id) as "count_fines",
        min(t.brand) as "brand", min(t."number") as "number_tr"
        from 
            fine f full join transport t on f.transport = t.id full join driver d on t."owner" = d.id
        group by d.id)	
    on l.driver = driver_id
) group by driver_id;

Результат запроса был выгружен в .csv файл для дальнейшего анализа.

Данные:

`
