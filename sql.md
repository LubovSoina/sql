## Базовые операции ##

1.Оператор **SELECT** осуществляет выборку из базы данных 

```mysql
select *
from labs.activity
```
2. Выбрать конкретные столбцы

```mysql
select 
  id,
  title
from 
  labs.activity
```

3. Простая сортировка
Сортировку можно проводить по возрастанию (параметр **ASC** принимается по умолчанию) или по убыванию (параметр **DESC**).

```mysql
select 
  title
from 
  labs.author
order by 
  title asc
```
4. Горизонтальную выборку реализует предложение **WHERE**, которое записывается после предложения **FROM**. При этом в результирующий набор 
попадут только те строки из источника записей, для каждой из которых значение **WHERE** равно **TRUE**. (Условие выборки запросов)
```mysql
select 
  title,
  isDeleted
from 
  labs.event
where 
  labs.event.isDeleted=0 
```

5.Добавим  переменные, чтобы было удобнее обращаться
```mysql
select 
  title,
  isDeleted
from  
  labs.event e
where 
  e.title is not null
```
6. Комбинация условий, построенная с помощью
булевых операторов **AND**, **OR** или **NOT**. Кроме того, в этих комбинациях может использоваться SQL-оператор **IS**, 
а также круглые скобки для конкретизации порядка выполнения операций.

```mysql
select 
  title,
  isDeleted
from 
  labs.event e
where 
  e.title is not null 
    and 
  isDeleted=0
```

```mysql
select 
  title,
  createDT
from 
  labs.event e

where 
  e.createDT > '2020-01-21'
```



```mysql
select *
from 
  labs.context c
where 
  c.id in (35,134,174)
```


```mysql
select *
from 
  labs.context c
where 
  c.id not in (1,2,3)
```




## Получение итоговых значений. Агрегация данных ##

```mysql
select 
  title,
  min(createDT)
from 
  labs.event e
```

```mysql
select 
   title,
   max(createDT)
from  
  labs.event e
```



2.Предложение **GROUP BY** используется для определения групп выходных строк, 
к которым могут применяться агрегатные функции (COUNT, MIN, MAX, AVG и SUM)

```mysql
select
       em.id,
       em.user_id,
       em.file,
       em.url
from isle.isle_eventmaterial em
```

Посчитаем сколько материалов на каждого пользователя:
```mysql
select
       em.user_id,
       count(em.id)

from isle.isle_eventmaterial em
group by em.user_id
```
Переименование заголовка с помощью ключевого слова **AS**
```mysql
select
       em.user_id,
       count(em.id) as 'количество цифровых следов'

from isle.isle_eventmaterial em
group by em.user_id
```



*Как найти среднее значение ?
Нельзя использовать подзапрос в качестве аргумента агрегатной функции.
Решение:
```sql
select avg(artefacts)
from (
         select em.user_id,
                count(em.id) as artefacts

         from isle.isle_eventmaterial em
         group by em.user_id
     ) X
```
## Запрос из нескольких таблиц ##
```mysql
select  distinct
       le.id,
       le.title,
       tm.file

from
    isle.isle_eventteammaterial tm
right join
    isle.isle_event ise
on
    ise.id = tm.event_id
left join
    labs.event le
on
    le.uuid = ise.uid

where le.id between  5920 and 5930
```

```mysql
select  distinct
       le.id,
       le.title,
       tm.file

from
    isle.isle_eventteammaterial tm
left join
    isle.isle_event ise
on
    ise.id = tm.event_id
left join
    labs.event le
on
    le.uuid = ise.uid

where le.id between  5920 and 5930
```


