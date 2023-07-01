# chapter 4 : 데이터 통합

<br>

## 1. sample table 생성
### 1️⃣ '설문 결과' 관련 간단한 table 생성 및 데이터 삽입
- table 생성 query

```mysql
CREATE TABLE `exampleSchema`.`inquiry` (
  `id` INT NOT NULL,
  `pref` VARCHAR(5) NULL,
  `age` INT NULL,
  `star` INT NULL,
  PRIMARY KEY (`id`));
```

- 데이터 삽입 query

```mysql
INSERT INTO `exampleSchema`.`inquiry` (`id`, `pref`, `age`, `star`) VALUES ('1', '서울시', '20', '2');
INSERT INTO `exampleSchema`.`inquiry` (`id`, `pref`, `age`, `star`) VALUES ('2', '충청도', '30', '5');
INSERT INTO `exampleSchema`.`inquiry` (`id`, `pref`, `age`, `star`) VALUES ('3', '경기도', '40', '3');
INSERT INTO `exampleSchema`.`inquiry` (`id`, `pref`, `age`, `star`) VALUES ('4', '충청도', '20', '4');
INSERT INTO `exampleSchema`.`inquiry` (`id`, `pref`, `age`, `star`) VALUES ('5', '서울시', '30', '4');
INSERT INTO `exampleSchema`.`inquiry` (`id`, `pref`, `age`, `star`) VALUES ('6', '서울시', '20', '1');
```

<br>

### 2️⃣ DISTINCT
- SELECT 된 내용 중 중복된 내용을 하나로 볼 수 있도록 도와주는 키워드
- SELECT를 통해 필요한 데이터를 가져오고, 그 데이터 중 중복을 제거! (순서가 중요하다는 의미)

<br>

### 3️⃣ 집계 함수
- COUNT 이외에도 SUM, AVG, MAX, MIN 등의 집계 함수가 존재
- 집계 함수는 SELECT, HAVING, ORDER BY의 3개의 키워드에만 활용될 수 있다 → WHERE 키워드에서 사용될 수 ❌
- 집계 함수를 통해 SELECT 하게 되는 경우, 하나의 레코드만을 출력함. 이 때, 집계 함수나 상수만이 SELECT 함수의 column으로 활용될 수 있다
    - 'SELECT id, COUNT(*)'과 같이 집계 함수와 일반 테이블의 column을 함께 가져오려는 시도는 안된다는 의미!
- 위의 집계 함수 중 COUNT만이 NULL 값도 연산에 포함

- sample table

|name|stock|
|:-|:-|
|샴푸|10|
|비누|20|
|NULL|NULL|

```mysql
SELECT COUNT(stock), SUM(stock) FROM 'sample table';
# COUNT(stock) = 3 → NULL인 마지막 row도 카운팅한다는 것을 알 수 있음
# SUM(stock) = 30 → NULL인 마지막 row를 제외하고 연산한다는 것을 알 수 있음
```

<br>

### 4️⃣ GROUP BY의 유의 사항
- group key에 NULL 값이 존재하는 경우 NULL도 하나의 그룹을 생성한다
- GROUP BY된 table에서 SELECT를 하는 경우, 집계 함수의 경우와 동일하게 하나의 레코드만을 출력할 수 있어야 한다. (단, group key로 사용된 컬럼 명은 활용 가능!)
- 여러 개의 group key가 활용된 경우 query에 작성된 순서대로 그룹화를 진행
- GROUP BY는 WHERE 키워드 다음에 실행됨 ⭐️⭐️⭐️

<br>

### 5️⃣ HAVING, group에 조건 주기
- 기본적인 내용은 Easy ㅎㅎ
- HAVING의 조건 판단은 group마다 시행된다는 것만 기억하자!

<br>

### 6️⃣ MySQL 키워드 실행 순서 ⭐️⭐️⭐️⭐️⭐️
- 기본적인 키워드 SELECT, DISTINCT, FROM, WHERE, GROUP BY, HAVING에 대해서...
- 1. FROM : table 호출
- 2. WHERE : 조건에 맞는 레코드 추출
- 3. GROUP BY : group key(s)를 기준으로 그룹화
- 4. HAVING : 그룹별로 조건에 맞는 레코드 추출
- 5. SELECT : 지금까지 저장된 데이터 가져옴
- 6. DISTINCT : SELECT에 의해 선택된 데이터에서 중복 제거
- 연산 순서를 명확히 알고 query를 작성해야 아래와 같은 실수를 안할 수 있겠지? (Tool에 의존하지 않는 습관은 필수이니 😊)

```mysql
SELECT pref as '지역' FROM inquiry GROUP BY '지역';
# query 작성 환경에 따라 오류가 안 날 수도 있겠지만...
# MySQL 키워드 실행 순서에 따르면 오류가 나야하는 query임!
```

<br>