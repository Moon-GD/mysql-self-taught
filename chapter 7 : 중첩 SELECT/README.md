# chapter 7 : 중첩 SELECT

<br>

## 1. main/subquery
### 1️⃣ 개념 숙지
- SELECT 키워드가 2개 이상 사용된 경우..
    - 외부 query := main query
    - 내부 query := subquery

<br>

## 2. 복수 레코드를 가져오는 subquery
### 1️⃣ 1개의 column, 여러 개의 레코드를 가져오는 경우
- 아래의 연산자를 활용할 수 있음

| 연산자 | 사용법 | 의미 |
|:-|:-|:-|
| IN | a IN (subquery) | subquery 결과 중 하나와 일치하는 경우 1 반환 |
| NOT IN | a NOT IN (subquery) | subquery 결과 중 어느 것과도 일치하지 않는 경우 1 반환 |
| ANY | a <임의 연산자> ANY (subquery) | 어떤 subquery와 a의 연산 결과가 1인 경우 1 반환 |
| ALL | a <임의 연산자> ANY (subquery) | 모든 subquery와 a의 연산 결과가 1인 경우 1 반환 |

- 활용 예제

```mysql
# membership 등급이 3 이상인 회원 정보
SELECT * 
FROM customer 
WHERE membertype_id IN (
    SELECT membertype_id from member WHERE membership >= 3
);

# 구매 이력이 없는 고객 정보
SELECT * 
FROM customer 
WHERE buy_date NOT IN (
    SELECT buy_date FROM buytable
);

# 월별 통신비 사용량이 한 번이라도 해당 월 프리미엄 조건을 넘은 적이 있는 고객 정보
SELECT *
FROM customer
WHERE month_pay >= ANY (
    SELECT premium_pay_condition
    FROM premium_condition_table
);

# 2023년의 모든 달에 평균 지출 금액을 초과한 고객 정보
SELECT * 
FROM customer
WHERE customer_month_pay > ALL (
    SELECT AVG(customer_month_pay)
    FROM customer
    GROUP BY month
);
```

<br>

### 2️⃣ NULL in subquery
- NULL 값이 존재하는 경우? 
    - 연산 결과가 1을 반환하는 경우 → 이상 ❌
    - 연산 결과가 0을 반환하는 경우 → NULL 반환
- 따라서 subquery에서 NULL 처리를 해주는 것이 바람직
- NULL handling 예제

```mysql
# premium rank가 3 이상인 고객 정보 가져오기
SELECT * 
FROM customer
WHERE customer_id IN (
    SELECT customer_id
    FROM premium_customer
    WHERE customer_id IS NOT NULL AND premium_rank >= 3
);
```

<br>

### 3️⃣ subquery가 table을 가져오는 경우
- 집계 함수와 GROUP BY를 활용하여 1개의 칼럼이 되도록 변경
- 이후 1️⃣번 과정과 동일하게 진행

<br>

## 3. correlated subquery
### 1️⃣ 정의
- 지금까지 배웠던 중첩 query는 sub query → main query의 순서
- 다만, sub query에서 main query의 값을 참조하는 경우를 correlated subquery(상관 서브 쿼리) 라고 함
- correlated subquery의 경우 main query의 레코드 하나마다 subquery가 실행됨! → 즉, main query가 먼저 실행됨!

<br>

### 2️⃣ 예제 - 문제 상황
- 고객 테이블, 상품 주문 테이블이 주어짐
- 고객 테이블 (customer)

| customer_id | customer_name |
|:-|:_|
| 1 | 김바람 |
| 2 | 이구름 |
| 3 | 박하늘 |
| 4 | 강산 |
| 5 | 유바다 |

- 상품 주문 테이블 (productorder)

| order_id | customer_id | product_id |
|:-|:-|:-|
| 1 | 4 | 1 |
| 2 | 5 | 3 |
| 3 | 2 | 2 |
| 4 | 3 | 2 |
| 5 | 1 | 4 |
| 6 | 5 | 2 |
| 7 | 1 | 5 |

- 두 테이블을 참조하여 고객별 총 주문량을 구하고 싶다면?

<br>

### 3️⃣ 예제 - 문제 해결
- 고객별 총 주문량을 구하기 위해서는...

```mysql
SELECT
    customer_id,
    customer_name,
    (
    
        SELECT COUNT(customer_id)
		FROM productorder
		GROUP BY customer_id
		HAVING customer.customer_id = productorder.customer_id
    ) AS '총 구매량'
FROM customer;
```

- 결과 테이블

| customer_id | customer_name | 총 구매량 |
|:-|:-|:-|
| 1 | 김바람 | 2 |
| 2 | 이구름 | 1 |
| 3 | 박하늘 | 1 |
| 4 | 강산 | 1 |
| 5 | 유바다 | 2 |

<br>

## 4. EXISTS 연산자
### 1️⃣ 정의
- 사용법 : EXISTS (subquery)
- 의미 : subquery에 결과가 존재하면 1을 반환

<br>

### 2️⃣ 예제 - 문제 상황
- productorder/product table 2개가 주어짐
- product 중 주문이 들어온 제품 정보를 확인하고자 함

<br>

### 3️⃣ 예제 - 문제 해결

```mysql
SELECT
    product_id,
    product_name
FROM product
WHERE 
    EXISTS (
        SELECT *
        FROM productorder
        WHERE product.product_id = productorder.product_id
    );
```

<br>