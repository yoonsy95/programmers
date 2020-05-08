> [프로그래머스](https://programmers.co.kr/learn/challenges)

#

## SUM, MAX, MIN

### 최솟값 구하기

```sql
-- mySQL
SELECT min(datetime) 시간 from animal_ins;

-- Oracle
-- Oracle에는 min 기능이 없어 서브 쿼리 사용함
SELECT datetime 시간 
from (select * 
      from animal_ins 
      order by datetime) 
where rownum=1;
```

### 동물 수 구하기

```sql
SELECT count(*) count 
from animal_ins
```

### 중복 제거하기

```sql
SELECT count(distinct name) count 
from animal_ins;
```

#

## GROUP BY

### 고양이와 개는 몇 마리 있을까

```sql
SELECT animal_type, count(animal_type) 
from animal_ins 
group by animal_type 
order by animal_type;
```

### 동명 동물 수 찾기

```sql
select name, count(*) 
from animal_ins 
where name is not null
group by name having count(*)>=2
order by name;
```

### 입양 시각 구하기(1)

```sql
-- mySQL
SELECT hour(datetime) hour, count(*) count
from animal_outs
group by hour 
having hour between 9 and 19;

-- Oracle
-- group by절은 alias 사용 불가
SELECT to_char(datetime,'hh24') as hour, count(*) count
from animal_outs
group by to_char(datetime,'hh24')
having to_char(datetime,'hh24') between 9 and 19
order by to_char(datetime,'hh24');

-- alias 사용 위하여 서브쿼리로 변경
SELECT ots.hour, count(ots.hour)
from(select extract(hour 
                    from cast(datetime as timestamp)) as hour
     from animal_outs) ots
where ots.hour between 9 and 19
group by ots.hour
order by ots.hour;
```



#

## NULL

### NULL 처리하기

```sql
-- mySQL
SELECT animal_type, ifnull(name,'No name') name, sex_upon_intake
from animal_ins
order by animal_id;

-- Oracle
SELECT animal_type, nvl(name,'No name'), sex_upon_intake
from animal_ins
order by animal_id;
```

#

## String, Date

### 루시와 엘라 찾기

```sql
SELECT animal_id, name, sex_upon_intake
from animal_ins
where name in ( 'Lucy', 'Ella', 'Pickle', 'Rogan', 'Sabrina', 'Mitty')
order by animal_id;
```

### 이름에 el이 들어가는 동물 찾기

```sql
-- mySQL
SELECT animal_id, name
from animal_ins 
where name like '%el%' and animal_type='dog'
order by name;

-- Oracle
-- mySQL은 대소문자 상관이 없으나
-- Oracle은 신경써줘야 
SELECT animal_id, name
from animal_ins 
where upper(name) like upper('%el%')  and animal_type='Dog'
order by name;
```

### 중성화 여부 파악하기

```sql
SELECT animal_id,
       name, 
       sex_upon_intake,
       case 
        when sex_upon_intake like 'Intact%' then 'X' 
        else 'O' 
       end
from animal_ins
order by animal_id;
```

### DATETIME에서 DATE로 형 변환 

```sql
-- mySQL
SELECT animal_id, name, date_format(datetime,'%Y-%m-%d') 날짜
from animal_ins
order by animal_id;

-- Oracle
SELECT animal_id, name, to_char(datetime,'yyyy-mm-dd') 날짜
from animal_ins
order by animal_id;
```

