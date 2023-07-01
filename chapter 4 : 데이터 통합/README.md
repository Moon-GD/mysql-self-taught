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

### 4️⃣ 
- 

<br>