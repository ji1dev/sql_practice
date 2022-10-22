# SQL 고득점 Kit (SUM, MAX, MIN) 풀이

<br/>

## 16 - 가장 비싼 상품 구하기
```sql
SELECT MAX(PRICE) AS MAX_PRICE
FROM PRODUCT
```

<br/>

## 17 - 가격이 제일 비싼 식품의 정보 출력하기
```sql
-- 방법 1. 가격순으로 정렬한 뒤 LIMIT 걸어서 출력
SELECT * FROM FOOD_PRODUCT
ORDER BY PRICE DESC
LIMIT 1
```

```sql
-- 방법 2. 서브쿼리로 가격 최댓값 뽑아내고, 해당 값을 가지는 레코드를 출력
SELECT * FROM FOOD_PRODUCT
WHERE PRICE = (SELECT MAX(PRICE) FROM FOOD_PRODUCT)
```

<br/>

## 18 - 최댓값 구하기
```sql
SELECT DATETIME AS 시간
FROM ANIMAL_INS
WHERE DATETIME = (SELECT MAX(DATETIME) FROM ANIMAL_INS)
```

<br/>

## 19 - 최솟값 구하기
```sql
SELECT DATETIME AS 시간
FROM ANIMAL_INS
WHERE DATETIME = (SELECT MIN(DATETIME) FROM ANIMAL_INS)
```

<br/>

## 20 - 동물 수 구하기
```sql
SELECT COUNT(*) AS count
FROM ANIMAL_INS
```

<br/>

## 21 - 중복 제거하기
```sql
SELECT COUNT(DISTINCT NAME) AS count
FROM ANIMAL_INS
```