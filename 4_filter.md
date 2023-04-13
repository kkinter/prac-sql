# 4.1 조건 평가
```
WHERE first_name = "STEVEN" AND create_date > "2006-01-01"
```
- 모두 True
```
WHERE first_name = "STEVEN" OR create_date > "2006-01-01"
```
## 4.1.1 괄호 사용
```sql
WHERE (first_name = 'STEVEN' OR last_name = 'YOUNG')  
AND create_date > '2006-01-01'
```

### 4.1.2 not 연산자 사용
```sql
WHERE NOT (first_name = 'STEVEN' OR last_name = 'YOUNG') 
AND create_date > '2006-01-01'
```
- 사람이 `NOT` 연산자를 포함하는 `WHERE` 절을 판단하기 어려울 수 있음

```sql
WHERE first_name <> 'STEVEN' AND last_name <> 'YOUNG'
AND create_date > '2006-01-01'
```

# 4.2 조건 작성
- 조건은 하나 이상의 연산자와 결합된 하나 이상의 표현식으로 구성
  - 숫자
  - 테이블 또는 뷰의 열
  - concat('Learing', ' ', 'SQL')과 같은 내장 함수
  - 서브쿼리
  - ('Boston', 'New York', 'Chicago') 와 같은 표현식 목록
- 연산자
  - =, !=, <, >, <>, like, in 및 between 과 같은 비교 연산자
  - +, -, * 및 / 과 같은 산술 연산자

# 4.3 조건 유형

### 4.3.1 동등조건
```sql
title = 'RIVER OUTLAW'
fed_id = '111-11-1111'
amount = 375.25
film_id = (SELECT film_id FROM film WHERE title = 'RIVER OUTLAW')
```
```sql
SELECT c.email
FROM customer c
  INNER JOIN rental r
  ON c.customer_id = r.customer_id
WHERE date(r.rental_date) = '2005-06-14';
```
```
+---------------------------------------+
| email                                 |
+---------------------------------------+
| CATHERINE.CAMPBELL@sakilacustomer.org |
| JOYCE.EDWARDS@sakilacustomer.org      |
| AMBER.DIXON@sakilacustomer.org        |
| JEANETTE.GREENE@sakilacustomer.org    |
| MINNIE.ROMERO@sakilacustomer.org      |
| GWENDOLYN.MAY@sakilacustomer.org      |
| SONIA.GREGORY@sakilacustomer.org      |
| MIRIAM.MCKINNEY@sakilacustomer.org    |
| CHARLES.KOWALSKI@sakilacustomer.org   |
| DANIEL.CABRAL@sakilacustomer.org      |
| MATTHEW.MAHAN@sakilacustomer.org      |
| JEFFERY.PINSON@sakilacustomer.org     |
| HERMAN.DEVORE@sakilacustomer.org      |
| ELMER.NOE@sakilacustomer.org          |
| TERRANCE.ROUSH@sakilacustomer.org     |
| TERRENCE.GUNDERSON@sakilacustomer.org |
+---------------------------------------+
16 rows in set (0.05 sec)
```
### 부등 조건
```sql
SELECT c.email
FROM customer c 
  INNER JOIN rental r  
  ON c.customer_id = r.customer_id    
WHERE date(r.rental_date) <> '2005-06-14';
```
```
| AUSTIN.CINTRON@sakilacustomer.org        |
| AUSTIN.CINTRON@sakilacustomer.org        |
| AUSTIN.CINTRON@sakilacustomer.org        |
| AUSTIN.CINTRON@sakilacustomer.org        |
| AUSTIN.CINTRON@sakilacustomer.org        |
| AUSTIN.CINTRON@sakilacustomer.org        |
| AUSTIN.CINTRON@sakilacustomer.org        |
| AUSTIN.CINTRON@sakilacustomer.org        |
| AUSTIN.CINTRON@sakilacustomer.org        |
+------------------------------------------+
16028 rows in set (0.02 sec)
```
### 동등조건을 사용한 데이터 변경
```sql
DELETE FROM rental
WHERE year(rental_date) = 2004;
```
```sql
DELETE FROM rental
WHERE year(rental_date) <> 2005 
AND year(rental_date) <> 2006;
```
- rental date가 2005년 또는 2006년이 아닐 때
```
NOTE_ 필자는 이 책에서 delete 및 update 문 예제를 작성할 때 행이 수정되지 않도록 각 구문을 쓰려고 합니다. 그러면 명령문을 실행할 때 데이터가 변경되지 않아 select 문의 결과가 항상 이 책에 표시된 것과 일치합니다.MySQL 세션은 기본적으로 자동 커밋 모드이므로(12장 참조), 작성한 구문 중 하나가 데이터를 수정했을 경우 예제 데이터의 변경사항을 롤백(실행 취소)할 수 없습니다. 물론 데이터를 정리하고 스크립트를 처음부터 다시 실행하여 테이블을 채우는 등 원하는 모든 작업을 수행할 수는 있지만, 필자는 웬만하면 그대로 두고 데이터를 원래대로 유지하려 합니다
```

## 4.3.2 범위조건
```sql
SELECT customer_id, rental_date
FROM rental
WHERE rental_date < '2005-05-25'
```
```sql
SELECT customer_id, rental_date
FROM rental
WHERE rental_date <= '2005-06-16'
AND rental_date >= '2005-06-14'
```
### between 연산자
- 상한과 하한 모두 있을 때
- 항상 범위의 하한을 지정하고, 상한을 두 번째로 지정해야함 
- 상한을 먼저 지정할 경우 Empty set 이 반환됨.
- 값들이 포함되어 반환 (<=, >=)
```sql
SELECT customer_id, rental_date
FROM rental
WHERE rental_date 
BETWEEN '2005-06-14' AND '2005-06-16';
```
### 문자열 범위
- 문자열 범위를 사용하려면 캐릭터셋의 순서를 알아야함 (순서 규칙을 콜레이션이라 함.)
```sql
SELECT last_name, first_name
FROM customer
WHERE last_name BETWEEN 'FA' AND 'FR';
```
```
+------------+------------+
| last_name  | first_name |
+------------+------------+
| FARNSWORTH | JOHN       |
| FENNELL    | ALEXANDER  |
| FERGUSON   | BERTHA     |
| FERNANDEZ  | MELINDA    |
| FIELDS     | VICKI      |
| FISHER     | CINDY      |
| FLEMING    | MYRTLE     |
| FLETCHER   | MAE        |
| FLORES     | JULIA      |
| FORD       | CRYSTAL    |
| FORMAN     | MICHEAL    |
| FORSYTHE   | ENRIQUE    |
| FORTIER    | RAUL       |
| FORTNER    | HOWARD     |
| FOSTER     | PHYLLIS    |
| FOUST      | JACK       |
| FOWLER     | JO         |
| FOX        | HOLLY      |
+------------+------------+
```
## 4.3.3 멤버십 조건
```sql
SELECT title, rating
FROM film
WHERE rating = 'G' OR rating = 'PG';
```
- OR 연산자로 두개의 조건을 그리 번거롭지 않지만 여러 개의 항목이 필요할 때
- IN 연산자를 사용할 수 있음
```sql
SELECT title, rating
FROM filmWHERE rating 
IN ('G','PG');
```
### not in 사용
- 집합 내 존재하지 않는지 여부
```sql
SELECT title, rating
FROM film
WHERE rating 
NOT IN ('PG-13','R', 'NC-17');
```
## 4.3.4 일치조건
```sql
SELECT last_name, first_name
FROM customer
WHERE left(last_name, 1) = 'Q';
```
- 성이 Q로 시작하는 고객
- left() 함수는 유연하지 못함

### 와일드카드 사용
- `-` 정확히 한 문자
- `%` 개수에 상관없이 모든 문자 (0 포함)
- `F%`

```sql
SELECT last_name, first_name
FROM customer
WHERE last_name 
LIKE '_A_T%S';
```
- 두 번째 위치에 A를 포함하고 네번 째 위치에 T를 포함하며
  마지막은 S로 끝나는 문자열
```
+-----------+------------+
| last_name | first_name |
+-----------+------------+
| MATTHEWS  | ERICA      |
| WALTERS   | CASSANDRA  |
| WATTS     | SHELLY     |
+-----------+------------+
```
```sql
-- F%F로 시작하는 문자열%
-- %t t로 끝나는 문자열
-- %bas%문자열 'bas'를 포함하는 문자열
-- _ _t_세 번째 위치에 t가 있는 4글자 문자열
-- _ _ _-_ _-_ _ _ _네 번째와 일곱 번째 위치에 –가 있는 11자리 문자열
SELECT last_name, first_name
FROM customer
WHERE last_name 
LIKE 'Q%' OR last_name LIKE 'Y%';
```
### 정규 표현식
```sql
-- 이름이 Q 또는 Y로 시작하는 모든 고객
mysql> SELECT last_name, first_name
FROM customer
WHERE last_name 
REGEXP '^[QY]';
```
# 4.4 Null
  - 해당사항 없음 not applicable   
      예를 들어 ATM 기계에서 발생한 거래내역의 직원 ID 열
  - 아직 알려지지 않은 값 value not yet known   
      예를 들어 고객 테이블에 행이 생성될 때 연방 ID를 알 수 없는 경우 
  - 정의되지 않은 값 value undefined   
      예를 들어 데이터베이스에 아직 추가되지 않은 제품의 계좌가 생성된 경우

```
NOTE_ 일부 이론가들은 이러한 (그리고 더 많은) 상황을 다루는 다른 표현이 필요하다고 주장하지만, 실무자들은 다수의 null 값을 갖는 게 오히려 더욱 혼란스러울 것이라는 의견에 동의할 것입니다
```
- Null 일 수는 있지만, null과 같을 수는 없습니다.
- 두 개의 null은 서로 같지 않습니다.
```sql
-- 표현식이 null 인지 확인
SELECT rental_id, customer_id
FROM rental
WHERE return_date IS NULL;
```
```sql
-- 위와 동일
SELECT rental_id, customer_id
FROM rental
WHERE return_date = NULL;
```


```sql
-- 5월과 8월 사이에 반납되지 않은 대여정보를 찾는다
SELECT rental_id, customer_id, return_date
FROM rental
WHERE return_date 
NOT BETWEEN '2005-05-01' 
AND '2005-09-01';
```
```sql
-- 올바르게 찾을 경우
SELECT rental_id, customer_id, return_date
FROM rental
WHERE return_date IS NULL OR return_date NOT BETWEEN '2005-05-01' AND '2005-09-01';
```
# 4.5 점검
```sql
-- 4-3
SELECT * 
FROM payment 
WHERE amount = 1.98 OR amount = 7.98 OR amount=9.98

;
```
```sql
SELECT *
FROM customer
WHERE last_name 
LIKE '_AW%';
LIMIT 10;
```