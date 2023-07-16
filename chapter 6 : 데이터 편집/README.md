# chapter 6 : 데이터 편집

<br>

## 1. CASE 키워드 활용 예제
### 1️⃣ 주어진 조건
- 아래와 같은 테이블이 있다고 가정
- delivery 테이블

|delivery_id|customer|quantity|delivery_time|
|:-|:-|:-|:-|
| 1 | A사 | 5 | 1 |
| 2 | B사 | 3 | 3 |
| 3 | C사 | 2 | 2 |
| 4 | D사 | 8 | NULL |
| 5 | E사 | 12 | 1 |

- 배송 요금표 테이블

| 개수 | 배송료 |
|:-|:-|
| 3개 이하 | 1,000 |
| 4 ~ 7개 | 1,200 |
| 8 ~ 10개  | 1,500 |
| 11개 이상 | 2,000 |

- 배송 시간대 테이블

| 배송 시간대 코드 | 내용 |
|:-|:-|
| 1 | 오전 |
| 2 | 오후 |
| 3 | 야간 |

<br>

### 2️⃣ 배송료 정보 편집 query

```mysql
SELECT 
    customer, 
    (CASE
        WHEN quantity < 0 THEN 0
        WHEN quantity <= 3 THEN 1000
        WHEN quantity <= 7 THEN 1200
        WHEN quantity <= 10 THEN 1500
        ELSE 2000
    END) AS 배송료
FROM
    delivery;
```

- 결과 테이블 

|customer|배송료|
|:-|:-|
| A사 | 1200 |
| B사 | 1000 |
| C사 | 1000 |
| D사 | 1500 |
| E사 | 2000 |

<br>

### 3️⃣ 배송 시간 편집

```mysql
SELECT
    customer,
    (CASE delivery_time
        WHEN 1 THEN '오전'
        WHEN 2 THEN '오후'
        WHEN 3 THEN '야간'
        ELSE '지정 없음'
    END) AS '배달 시간'
FROM
    delivery;
```

- 결과 테이블

|customer|배달 시간|
|:-|:-|
| A사 | 오전 |
| B사 | 야간 |
| C사 | 오후 |
| D사 | 지정 없음 |
| E사 | 야간 |

<br>

## 2. IF 키워드 활용 예제
### 1️⃣ quantity에 따른 사은품 편집
- 5개 초과로 구매한 경우 사은품 증정

```mysql
SELECT 
    customer,
    IF(quantity > 5, 'O', 'X') AS '사은품 증정 여부'
FROM delivery;
```

- 결과 테이블

|customer|사은품 증정 여부|
|:-|:-|
| A사 | X |
| B사 | X |
| C사 | X |
| D사 | O |
| E사 | O |

<br>

## 3. NULL handling
### 1️⃣ COALESCE 키워드란?
- 여러 개의 인수를 가질 수 있는 MySQL 함수
- 인자 순서대로 값을 비교하며 NULL이 아닌 값을 반환
- 활용 예제

```mysql
# 1 반환
COALESCE(1)

# 2 반환
COALESCE(NULL, 2)

# 'abc' 반환
COALESCE(NULL, NULL, 'abc', NULL)

# NULL 반환
COALESCE(NULL, NULL)
```

<br>

### 2️⃣ NULL 값 대체 예제
- 위의 배달표 중 비어 있는 값을 대체하고 배송 시간 편집

```mysql
SELECT
    (CASE COALESCE(delivery_time, 4)
        WHEN 1 THEN '오전'
        WHEN 2 THEN '오후'
        WHEN 3 THEN '야간'
        WHEN 4 THEN '지정 없음'
    END) AS '배송 시간',
    customer
FROM
    delivery
ORDER BY COALESCE(delivery_time, 4);
```

- 결과 테이블

| customer | 배송 시간 |
|:-|:-|
| 오전 | A사 |
| 오전 | E사 |
| 오후 | C사 |
| 야간 | B사 |
| 지정 없음 | D사 |

<br>

### 3️⃣ IFNULL 키워드
- 2개의 인자를 받는 함수
- 첫 번째 값이 NULL이 아니라면? 첫 번째 값 반환
- 첫 번째 값이 NULL이라면? 두 번째 값 반환

<br>

## 4. NULL 반환
### 1️⃣ NULLIF 키워드
- 경우에 따라 NULL 반환을 통해 부적절한 연산을 방지하는 것이 필요 (ex : 0으로 나누기)
- 2개의 인자를 받으며, 인자 값이 서로 같은 경우 NULL을 반환
- 예제

```mysql
# NULL 값 반환
NULLIF(0, 0)

# 첫 번째 인자 반환 → 30 반환
NULLIF(30, 10)

```

<br>

## 5. 타입 변환 (Casting)
### 1️⃣ 기본적인 활용 방법
- format : CAST(데이터 AS 새로운 데이터 형)
- 예제

```mysql
# 123 + 1인 124 반환
CAST('123' AS SIGNED) + 1

# 3.14159 반환
CAST('3.14159' AS DECIMAL(6, 5))

```

- 변환되는 데이터 목록

|데이터 타입|사용 방법|의미|
|:-|:-|:-|
| BINARY |BINARY|바이너리|
|  |BINARY(a)|a 바이트의바이너리|
| CHAR | CHAR | 문자 |
|  | CHAR(a) | a 길이의 문자(길이가 부족하면 뒤에 공백 추가) |
| DECIMAL | DECIMAL | 소수 |
|  | DECIMAL(a) | a 자리의 소수 |
|  | DECIMAL(a, b) | 전체 길이 a, 소수부 b 자리의 소수 |
| SIGNED | SIGNED, SIGNED INTEGER | 부호 있는 정수 |
| UNSIGNED | UNSIGNED, UNSIGNED INTEGER | 부호 없는 정수 |

<br>

### 2️⃣ 활용 예제
- scoretable (교내 성적 테이블)

| student_id | name | score |
|:-|:-|:-|
| 1 | A | 90 |
| 2 | B | 9 |
| 3 | C | 100 |
| 4 | D | 70 |

- 성적 순서대로 등수를 매긴다면?

```mysql
SELECT
    name,
    score
FROM 
    scoretable
ORDER BY CAST(score AS SIGNED) DESC;
```

- 정렬 결과 테이블

| name | score |
|:-|:-|
| C | 100 |
| A | 90 |
| D | 70 |
| B | 9 |

<br>