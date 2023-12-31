# chapter 2 : 특정 데이터를 가져오자

<br>

## 1. 문자열 비교
### 1️⃣ = 연산자를 활용한 문자열 동등 비교의 문제점
- '=' 연산자를 활용한 문자열 비교는 2가지 문제점을 지님
    - 1. 대소문자 무시 → 'a' = 'A'  # true
    - 2. 끝의 공백 무시 → 'b' = 'b '  # true (다만, 이 경우 환경에 따라 툴이 끝의 공백을 잘라주는 경우도 있음)
- 따라서 대소문자를 구분하여 정확한 동등 비교를 원하는 경우 BINARY 예약어를 활용하면 된다
- BINARY 사용 전

<img width="285" alt="BINARY 사용 전" src="https://github.com/Moon-GD/mysql-self-taught/assets/74173976/5855f73c-5d81-451c-b87f-5593e411b6cc">

<br>

- BINARY 사용 후

<img width="290" alt="BINARY 사용 후" src="https://github.com/Moon-GD/mysql-self-taught/assets/74173976/81a3dfe9-92fe-4416-a4ba-ba9eaed4fea7">
<br><br>

## 2. 문자열 포함
### 1️⃣ LIKE 연산자와 함께 사용되는 '_' & '%'
- '_' : 임의의 문자 1개를 의미
- '%' : 0개 이상의 임의의 문자열을 의미
```mysql
# 서울에 거주하는 성이 김이고 3글자 이름을 가지는 사람의 정보를 모두 찾아라
select * from testTable where 
    name LIKE '김__' AND
    address LIKE '서울%';
```

<br>
