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

- 결과 값 

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

- 결과 값

|customer|배달 시간|
|:-|:-|
| A사 | 오전 |
| B사 | 야간 |
| C사 | 오후 |
| D사 | 지정 없음 |
| E사 | 야간 |

<br>