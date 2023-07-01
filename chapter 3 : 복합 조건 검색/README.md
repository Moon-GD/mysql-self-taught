# chapter 3 : 복합 조건 검색

<br>

## 1. 조건 검색 관련 키워드
### 1️⃣ BETWEEN
- 'x이상 y사이의 price'라는 조건을 줄 때 Between을 활용하는 것이 더 직관적! 생각하는 수고로움을 덜어준다
- BETWEEN 활용 전

```mysql
WHERE x <= price AND price <= y;
```

- BETWEEN 활용 후

```mysql
WHERE price BETWEEN x AND y;
```

- 되게 간단한 연산자이지만 개인적으로 활용해 본 기억이 없는 것 같아 정리해 봄! 😁

<br>

### 2️⃣ IN
- OR 연산의 반복이 길어질 때 활용할 수 있는 연산자. 이 또한 가독성을 위해 활용될 수 있다
- 예를 들어, 'age가 10대, 20대, 30대'인 조건을 주고자 한다면...
- IN 활용 전

```mysql
WHERE age = '10대' OR age = '20대' OR '30대';
```

- IN 활용 후

```mysql
WHERE age IN ('10대', '20대', '30대');
```

- 단, NULL 값에 유의할 필요가 있음! 아래의 예제가 NULL이 올 수 있는 모든 경우를 표현하니 참고할 것!

```mysql
# 비교 대상 : null
SELECT 1 IN (1, 2);  # 1
SELECT 1 IN (1, NULL);  # 1
SELECT 1 IN (NULL, 2);  # NULL

# 비교 대상 : not null
SELECT NULL IN (1);  # NULL
SELECT NULL IN (NULL); # NULL
```

- 이 또한, 활용해보지 않았기 때문에 정리! 😁

<br>

### 3️⃣ MySQL 나눗셈 연산에 대해서..
- 4가지 연산이 존재함을 처음 알았다
    - 1. '/' : 일반적인 나누기
    - 2. DIV : 정수 부분만을 반환
    - 3. MOD : modulo 연산
    - 4. '%' : modulo 연산
    <br>
- 또한, 다음의 경우에 모든 나눗셈 연산은 NULL을 반환한다
    - 1. 피연산자에 NULL이 포함된 경우
    - 2. 0으로 나눗셈을 시도하는 경우
    <br>
- 평소에 무의식으로 '음~ 그렇구나'하고 지나치던 것들 중에서도 모르는 연산들이 적지 않게 있었던 것 같다 🧐

<br>

### 4️⃣ SQL 모드
- 데이터 유형 일치, 특정 타입의 데이터 입력 불가, 날짜 데이터의 포맷 등의 규칙을 개발자가 작성해두는 것
- 스키마 혹은 테이블 단위로 적용이 가능하다

<br>
