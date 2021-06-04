# 집계 함수

최대, 최소, 합, 개수, 평균을 구하는 함수를 사용할 수 있습니다.

<BR>

## MAX, MIN, COUNT, SUM, AVG

각각 속성의 최대, 최소, 개수를 찾는 함수입니다.

<BR>

문제 출처: https://programmers.co.kr/learn/courses/30/lessons/59415

```SQL
SELECT MAX(DATETIME)
FROM ANIMAL_INS
```

<BR>

문제 출처: https://programmers.co.kr/learn/courses/30/lessons/59038

```SQL
SELECT MIN(DATETIME)
FROM ANIMAL_INS
```

<BR>

문제 출처: https://programmers.co.kr/learn/courses/30/lessons/59406

```SQL
SELECT COUNT(*)
FROM ANIMAL_INS
```

<BR>

COUNT의 경우 중복을 제거한 뒤 개수를 셀 수 있습니다.

문제 출처: https://programmers.co.kr/learn/courses/30/lessons/59408

```SQL
SELECT COUNT(DISTINCT NAME)
FROM ANIMAL_INS
WHERE NOT NAME IS NULL
```

<BR>

SUM과 AVG도 위와 같은 방법으로 사용할 수 있습니다.