# SQL 고득점 Kit (GROUP BY) 풀이

<br/>

## 22 - 진료과별 총 예약 횟수 출력하기
```sql
SELECT MCDP_CD AS 진료과코드,
        COUNT(*) AS 5월예약건수
FROM APPOINTMENT
WHERE YEAR(APNT_YMD) = 2022
        AND MONTH(APNT_YMD) = 5
GROUP BY MCDP_CD
ORDER BY 5월예약건수 ASC, MCDP_CD ASC
```

<br/>

## 23 - 즐겨찾기가 가장 많은 식당 정보 출력하기
```sql
SELECT A.FOOD_TYPE, A.REST_ID, A.REST_NAME, A.FAVORITES
FROM REST_INFO AS A
JOIN (SELECT FOOD_TYPE, MAX(FAVORITES) AS MAX_FAV
        FROM REST_INFO
        GROUP BY FOOD_TYPE) AS B
WHERE A.FOOD_TYPE = B.FOOD_TYPE AND A.FAVORITES = B.MAX_FAV
ORDER BY FOOD_TYPE DESC
```

<br/>

## 24 - 식품분류별 가장 비싼 식품의 정보 조회하기
```sql
-- 처음 AC 받은 쿼리 
SELECT A.CATEGORY, MAX_PRICE, A.PRODUCT_NAME
FROM FOOD_PRODUCT AS A
JOIN (SELECT CATEGORY, MAX(PRICE) AS MAX_PRICE
      FROM FOOD_PRODUCT
     GROUP BY CATEGORY) AS B
WHERE A.CATEGORY = B.CATEGORY
    AND A.PRICE = B.MAX_PRICE
    AND (A.CATEGORY = "과자"
    OR A.CATEGORY = "국"
    OR A.CATEGORY = "김치"
    OR A.CATEGORY = "식용유")
ORDER BY MAX_PRICE DESC
```
```sql
-- WHERE절에서 서브쿼리를 사용한 방법
SELECT CATEGORY, PRICE AS MAX_PRICE, PRODUCT_NAME
FROM FOOD_PRODUCT
WHERE PRICE IN (SELECT MAX(PRICE)
              FROM FOOD_PRODUCT
              GROUP BY CATEGORY)
    AND CATEGORY IN("과자", "국", "김치", "식용유")
ORDER BY MAX_PRICE DESC
```

<br/>

## 25 - 고양이와 개는 몇 마리 있을까
```sql
SELECT ANIMAL_TYPE, COUNT(*)
FROM ANIMAL_INS
WHERE ANIMAL_TYPE IN ("Cat", "Dog")
GROUP BY ANIMAL_TYPE
ORDER BY ANIMAL_TYPE ASC
```

<br/>

## 26 - 동명 동물 수 찾기
```sql
SELECT NAME, COUNT(*) AS COUNT
FROM ANIMAL_INS
WHERE NAME IS NOT NULL
GROUP BY NAME
HAVING COUNT >= 2
ORDER BY NAME ASC
```

<br/>

## 27 - 입양 시각 구하기(1)
```sql
SELECT HOUR(DATETIME) AS HOUR, COUNT(*) AS COUNT
FROM ANIMAL_OUTS
WHERE HOUR(DATETIME) >= 9 AND HOUR(DATETIME) <= 19
GROUP BY HOUR
ORDER BY HOUR ASC
```

<br/>

## 28 - 년, 월, 성별 별 상품 구매 회원 수 구하기
```sql
SELECT YEAR(SALES_DATE) AS YEAR,
    MONTH(SALES_DATE) AS MONTH,
    GENDER,
    COUNT(DISTINCT USER_ID) AS USERS
FROM USER_INFO
JOIN ONLINE_SALE USING(USER_ID)
WHERE GENDER IS NOT NULL
GROUP BY YEAR, MONTH, GENDER
ORDER BY YEAR ASC, MONTH ASC, GENDER ASC
```

<br/>

## 29 - 입양 시각 구하기(2)
```sql
WITH RECURSIVE HOUR_TBL AS (
    # 0~23 까지 값 들어있는 가상 테이블
	SELECT 0 AS HOUR
	UNION ALL
	SELECT HOUR+1 FROM HOUR_TBL WHERE HOUR<23
)
SELECT A.HOUR, COUNT(B.HOUR) AS COUNT
FROM HOUR_TBL AS A
LEFT JOIN (
    # ANIMAL_OUTS에서 각 튜플의 시간 정보만 추출한 테이블
    SELECT HOUR(DATETIME) AS HOUR
    FROM ANIMAL_OUTS
) AS B ON A.HOUR = B.HOUR
GROUP BY A.HOUR
ORDER BY A.HOUR ASC
```

<br/>

## 30 - 가격대 별 상품 개수 구하기
```sql
-- FLOOR() 함수로 구간을 나누는 방법
SELECT FLOOR(PRICE/10000)*10000 AS PRICE_GROUP,
    COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY PRICE_GROUP
ORDER BY PRICE_GROUP ASC
```

```sql
-- TRUNCATE() 함수로 구간을 나누는 방법
SELECT TRUNCATE(PRICE, -4) AS PRICE_GROUP,
    COUNT(*) AS PRODUCTS
FROM PRODUCT
GROUP BY PRICE_GROUP
ORDER BY PRICE_GROUP ASC
```