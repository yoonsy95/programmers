> [프로그래머스](https://programmers.co.kr/learn/challenges)

#

## SELECT

### 모든 레코드 조회하기

```sql
SELECT * 
from animal_ins 
order by animal_id;
```

### 역순 정렬하기

```sql
SELECT name, datetime 
from animal_ins 
order by animal_id desc;
```

### 아픈 동물 찾기

```sql
-- mySQL 같은 경우 'sick'처럼 소문자만 사용해도 되지만
-- Oracle은 데이터와 같게 입력해주어야 함
SELECT animal_id, name 
from animal_ins 
where intake_condition like 'Sick' 
order by animal_id;
```

### 어린 동물 찾기

```sql
SELECT animal_id, name 
from animal_ins 
where intake_condition <> 'Aged' 
order by animal_id;
```

### 동물 아이디와 이름

```sql
SELECT animal_id, name 
from animal_ins 
order by animal_id;
```

### 여러 기준으로 정렬하기

```sql
SELECT animal_id, name, datetime
from animal_ins
order by name, datetime desc;
```

### 상위 n개 레코드

```sql
-- mySQL
SELECT name 
from animal_ins 
order by datetime 
limit 1;

-- Oracle
-- mySQL처럼 limit 역할을 해 주는 명령어 없음
-- 서브쿼리로 정렬 후 rownum으로 가져와야 함
-- rownum
--   하나의 레코드만 호출할 경우 = 사용 가능하지만
--   두 개 이상의 레코드를 호출하는 경우 사용 불가함
--   >, >= 만 사용 가능
SELECT *
from (select name 
      from animal_ins 
      order by datetime)
where rownum<2;
```

#

## SUM, MAX, MIN

### 최대값 구하기

```sql
-- mySQL
SELECT max(datetime) 시간 from animal_ins;

-- Oracle
-- Oracle에는 max 기능 없음
SELECT datetime 시간 
from (select * 
      from animal_ins 
      order by datetime desc) 
where rownum=1;
```

#



## ISNULL

### 이름이 없는 동물의 아이디

```sql
SELECT animal_id
from animal_ins
where name is null
order by animal_id;
```

### 이름이 있는 동물의 아이디

```sql
SELECT animal_id
from animal_ins
where name is not null
order by animal_id;
```

