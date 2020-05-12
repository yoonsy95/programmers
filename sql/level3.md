## JOIN

### 없어진 기록 찾기

```sql
SELECT animal_id, name
from animal_outs
where animal_id not in (select animal_id 
                        from animal_ins)
order by animal_id;
```

### 있었는데요 없었습니다

```sql
SELECT i.animal_id, i.name
from animal_ins i, animal_outs o
where i.animal_id=o.animal_id and i.datetime>o.datetime
order by i.datetime;
```

### 오랜기간 보호한 동물(1)

```sql
-- mySQL
SELECT name, datetime
from animal_ins
where animal_id not in (select animal_id 
                        from animal_outs)
order by datetime
limit 3;

-- Oracle
-- Oracle은 무조건 전체 감싼 후(!!!) where 절에 rownum 사용
select *
from (SELECT name, datetime
      from animal_ins
      where animal_id not in (select animal_id 
                              from animal_outs)
      order by datetime)
where rownum<=3;
```





## String, Date

### 오랜 기간 보호한 동물(2)

```sql
-- mySQL
SELECT o.animal_id, o.name
from animal_outs o
join animal_ins i on i.animal_id=o.animal_id
order by (i.datetime - o.datetime)
limit 2;

-- Oracle
select *
from (
    SELECT o.animal_id, o.name
    from animal_outs o
    join animal_ins i on i.animal_id=o.animal_id
    order by (i.datetime - o.datetime))
where rownum<3;
```

