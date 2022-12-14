# SQL 고득점 Kit (JOIN) 풀이

<br/>

## 36 -그룹별 조건에 맞는 식당 목록 출력하기
```sql
SELECT MEMBER_NAME,
    REVIEW_TEXT,
    DATE_FORMAT(REVIEW_DATE, "%Y-%m-%d") AS REVIEW_DATE
FROM MEMBER_PROFILE
NATURAL JOIN REST_REVIEW
WHERE MEMBER_ID = (SELECT MEMBER_ID
                   FROM REST_REVIEW
                   GROUP BY MEMBER_ID
                   ORDER BY COUNT(*) DESC
                   LIMIT 1)
ORDER BY REVIEW_DATE ASC, REVIEW_TEXT ASC
```

<br/>

## 37 - 5월 식품들의 총매출 조회하기
```sql
SELECT PRODUCT_ID,
    PRODUCT_NAME,
    SUM(AMOUNT)*PRICE AS TOTAL_SALES
FROM FOOD_PRODUCT
NATURAL JOIN FOOD_ORDER
WHERE YEAR(PRODUCE_DATE) = 2022
    AND MONTH(PRODUCE_DATE) = 5
GROUP BY PRODUCT_ID
ORDER BY TOTAL_SALES DESC, PRODUCT_ID ASC
```

<br/>

## 38 - 없어진 기록 찾기
```sql
SELECT B.ANIMAL_ID, B.NAME
FROM ANIMAL_INS AS A
RIGHT JOIN ANIMAL_OUTS AS B ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE A.ANIMAL_ID IS NULL
ORDER BY B.ANIMAL_ID ASC
```

<br/>

## 39 - 있었는데요 없었습니다
```sql
SELECT A.ANIMAL_ID, A.NAME
FROM ANIMAL_INS AS A
JOIN ANIMAL_OUTS AS B ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE A.DATETIME > B.DATETIME
ORDER BY A.DATETIME ASC
```

<br/>

## 40 - 오랜 기간 보호한 동물(1)
```sql
SELECT A.NAME, A.DATETIME
FROM ANIMAL_INS AS A
LEFT JOIN ANIMAL_OUTS AS B ON A.ANIMAL_ID = B.ANIMAL_ID
WHERE B.ANIMAL_ID IS NULL
ORDER BY A.DATETIME ASC
LIMIT 3
```

<br/>

## 41 - 보호소에서 중성화한 동물
```sql
SELECT A.ANIMAL_ID, A.ANIMAL_TYPE, A.NAME
FROM ANIMAL_INS AS A
JOIN ANIMAL_OUTS AS B USING(ANIMAL_ID)
WHERE SEX_UPON_INTAKE <> SEX_UPON_OUTCOME
ORDER BY A.ANIMAL_ID
```

<br/>

## 42 - 상품 별 오프라인 매출 구하기
```sql
SELECT PRODUCT_CODE,
    SUM(SALES_AMOUNT)*PRICE AS SALES
FROM PRODUCT
NATURAL JOIN OFFLINE_SALE
GROUP BY PRODUCT_ID
ORDER BY SALES DESC, PRODUCT_CODE ASC
```

<br/>

## 43 - 상품을 구매한 회원 비율 구하기
```sql
SELECT YEAR(SALES_DATE) AS YEAR,
    MONTH(SALES_DATE) AS MONTH,
    COUNT(DISTINCT USER_ID) AS PURCHASED_USERS,
    ROUND(COUNT(DISTINCT USER_ID)/(SELECT COUNT(*)
                                   FROM USER_INFO
                                   WHERE YEAR(JOINED) = 2021), 1) AS PURCHASED_RATIO
FROM USER_INFO AS A
NATURAL JOIN ONLINE_SALE AS B
WHERE YEAR(JOINED) = 2021
GROUP BY YEAR, MONTH
ORDER BY YEAR ASC, MONTH ASC
```