# 그룹 함수

데이터들을 원하는 그룹으로 나누고 조건을 설정할 수 있습니다.

> GROUP BY [속성명]
>
> HAVING [그룹 함수 포함 조건]

GROUP BY에 포함된 것만 SELECT에 같이 사용할 수 있습니다.

<BR>

## Example

문제: https://programmers.co.kr/learn/courses/30/lessons/59040 

```SQL
SELECT ANIMAL_TYPE, COUNT(ANIMAL_TYPE) as count
FROM ANIMAL_INS
GROUP BY ANIMAL_TYPE
ORDER BY ANIMAL_TYPE
```

<br>

문제: https://programmers.co.kr/learn/courses/30/lessons/59041

```SQL
SELECT NAME, COUNT(NAME) AS COUNT
FROM ANIMAL_INS
WHERE NOT NAME IS NULL
GROUP BY NAME
HAVING COUNT(*) > 1
ORDER BY NAME
```

