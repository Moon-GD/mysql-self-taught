# chapter 5 : 데이터 정렬

<br>

## 1. ORDER BY
### 0️⃣ 정렬 default settings
- ASC가 default 임을 명시하면 됨! → 쉬워서 그냥 넘어갈 수도 있지만, 논리 값 판정에서 의외로 요긴하게 활용됨
- 2️⃣번 살펴보기

<br>

### 1️⃣ 정렬 키에 번호 활용하기 (알아만 두면 됨!)
- SELECT 구에서 지정한 Column을 1, 2, 3 ... 의 숫자로 정렬 키 활용 가능

```mysql
# 위 아래 구문은 동일!
SELECT product_name, stock FROM product ORDER BY 1;
SELECT product_name, stock FROM product ORDER BY product_name;
```

- 딱 보면 알겠지만 비-직관적이라 추천되지 않겠지?
- ORDER BY는 지금까지 나온 키워드 중 마지막에 실행됨!

<br>

### 2️⃣ 정렬에서 NULL 값 처리
- 처리계에 따라 NULL 값은 처음, 마지막으로 정렬됨
- 일반적으로 NULL 값은 제외하고 판정하는 경우가 많기에, 마지막 행에 확정적으로 놓고 싶을 때가 많음
- 그럴 때는 아래와 같이 논리 값 판정을 통해 정렬하면 됨 → 논리 값 판정의 결과 값은 0 또는 1이기에 NULL 값을 마지막에 오도록 하는 방법!

```mysql
# stock 값이 NULL인 경우 논리 값 판정에서 1의 값을 가짐
# 정렬은 기본적으로 ASC(오름차순)이기에 1의 값을 가지는 행들은 아래 쪽에 위치하게 됨
SELECT * FROM product ORDER BY stock IS NULL;
```

<br>

### 3️⃣ ORDER BY에 ALIAS 활용 
- 직관과는 다르게 ORDER BY에 홑/쌍 따옴표를 통해 별명을 감싸면 정렬이 잘 되지 않을 수 있음!

```mysql
# 올바른 경우
SELECT stock AS 가격 FROM product ORDER BY 가격;

# 올바르지 않은 경우
SELECT stock AS '가격' FROM product ORDER BY '가격';
```

### 4️⃣ collation order (대조 순서)
- 일반적인 대조 순서 : utf8_general_ci
- 대표적인 대조 순서 : utf8_unicode_ci, utf8_bin

|general_ci|unicode_ci|bin|
|:-|:-|:-|
|1|1|1|
|A|A|A|
|a|a|B|
|ab|ab|a|
|B|B|ab|

<br>

## 2. 행 지정
### 0️⃣ 유의할 점
- 앞으로 나오게 될 키워드 LIMIT, OFFSET은 표준 SQL에는 존재하지 않음
- MySQL에서만 사용되는 키워드임을 명시하고, 다른 DB를 활용할 때는 비슷한 기능의 다른 키워드를 활용할 것

<br>

### 1️⃣ LIMIT 기본적인 사용법
- 다루는 테이블의 크기가 큰 경우 일부 행만 가져와야 될 때가 있음!
- 이 때, LIMIT를 통해 일부 행만 가져올 수 있음

```mysql
# 첫 3행만 가져옴
SELECT * FROM product LIMIT 3;
```

- Workbench 사용 중이랴면 script 우측 상단에 LIMIT 옵션 설정 버튼을 통해 기본 LIMIT 값 설정 가능

<br>

### 2️⃣ OFFSET 기본적인 사용법
- 시작하는 행의 위치를 지정!
- 첫 행은 0에서부터 시작
- 예시 코드

```mysql
# 6번째 행에서부터 3개의 행을 가져와서 보여주는 query
SELECT * FROM product LIMIT 3 OFFSET 5;
```

- LIMIT 키워드 만을 활용하여 OFFSET 지정 가능

```mysql
# 위의 예제 코드와 같은 역할을 수행하는 query
SELECT * FROM product LIMIT 5, 3;
```

### 3️⃣ LIMIT, OFFSET 실행 순서
- 자연스럽게 OFFSET → LIMIT 순서임! Too Easy~ 😊

<br>