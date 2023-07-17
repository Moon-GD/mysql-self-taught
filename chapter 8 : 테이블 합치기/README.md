# chapter 8 : 테이블 합치기

<br>

## 1. 세로 합치기
### 1️⃣ 조건
- 전체 column의 수와 연관된 column의 타입이 같아야 함
- 위의 조건을 만족하지만 column의 이름이 다른 경우는?
    - → 첫 번째 table의 column 명을 따름

<br>

### 2️⃣ UNION, 세로 합치기
- 위의 조건을 만족하는 2개의 테이블을 세로로 합쳐주는 연산자
- 활용 예시

```mysql
# tableA, tableB를 세로로 붙이는 query
SELECT 
    * 
FROM tableA
UNION
SELECT
    *
FROM tableB;
```

- UNION은 중복되는 레코드는 생략시킴. 즉, 집합 연산임
- 중복된 레코드 모두를 보고 싶다면? → UNION ALL 활용!

<br>

### 3️⃣ 활용 예시 - 문제 상황
- 2021년, 2022년 설문 조사 테이블이 존재 (전체 column의 수 동일 & 연관된 column 타입 일치)
- 두 테이블을 기반으로 age가 20이상인 통합 설문 조사의 결과를 확인하고 싶음!

<br>

### 4️⃣ 활용 예시 - 문제 해결
- 문제 해결 query

```mysql
SELECT
    *
FROM (
    SELECT 
        * 
    FROM inquiry_2021
    UNION
    SELECT 
        *
    FROM inquiry_2022
) AS integrated_inquiry
WHERE integrated_inquiry.age >= 20;
```

<br>

## 2.가로 합치기
### 1️⃣ JOIN 연산자
- ON 키워드의 조건에 맞추어 두 테이블을 합쳐주는 연산자
- 활용 방법

```mysql
# 두 테이블에 공통으로 존재하는 some_column을 기준으로 가로로 합쳐주는 query
SELECT
    *
FROM 
    tableA
JOIN
    table B
ON tableA.some_column = tableB.some_column;
```

- JOIN은 INNER JOIN의 약자
- ON 조건에 맞는 레코드가 tableB에 존재하지 않는 경우 tableA의 레코드도 버림

<br>

### 2️⃣ OUTER JOIN 연산자
- LEFT OUTER JOIN, RIGHT OUTER JOIN이 존재
- 보통 LEFT JOIN, RIGHT JOIN으로 축약하여 작성
- LEFT/RIGHT는 기준이 되는 테이블을 지정하는 키워드
- 기준 테이블에 대응되는 레코드가 존재하지 않는 경우 NULL 값을 넣어서 가로로 합침

<br>

### 3️⃣ CROSS JOIN 연산자
- a CROSS JOIN b : 테이블 a / b의 모든 레코드 조합을 합침 
    - → 모든 조건을 합치기에 ON 연산자 불필요!
- 대응되는 레코드 값이 존재하지 않는 경우 NULL 값 대입

<br>

### 4️⃣ USING 키워드
- 테이블 결합 조건이 ON a.some_column = b.some_column과 같이 = 연산자를 활용하는 경우, USING 키워드로 대체 가능

```mysql
# 위 아래 두 query는 동일한 작업을 수행
ON a.some_column = b.some_column
USING (some_column)
```

- USING 뒤에 오는 column 이름은 꼭 괄호 안에 작성해주어야 함

<br>