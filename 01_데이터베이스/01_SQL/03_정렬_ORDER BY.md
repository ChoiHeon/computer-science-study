# 정렬

특정 속성을 기준으로 정렬할 수 있습니다.

> ORDER BY 속성명 (ASC / DESC)

속성명을 기준으로 정렬하되, ASC는 기본 정렬, DESC는 역정렬입니다.
둘을 생략할 수 도 있으며 자동으로 기본 정렬로 지정됩니다.

<BR>

## Example 1

문제 출처: https://programmers.co.kr/learn/courses/30/lessons/59035

```sql
SELECT NAME, DATETIME
FROM ANIMAL_INS
ORDER BY ANIMAL_ID DESC
```

<BR>

## 다중 정렬

정렬 조건을 여러 개 주려면 다음과 같이 하면 됩니다.

> ORDER BY 속성명1 (ASC, DESC), 속성명2 (ASC, DESC), ...

앞에 있는 속성으로 정렬한 뒤, 만약 속성값이 같다면 뒤에 있는 속성으로 정렬합니다.

<BR>

## Example 2

문제 출처: https://programmers.co.kr/learn/courses/30/lessons/59404

```SQL
SELECT ANIMAL_ID, NAME, DATETIME
FROM ANIMAL_INS
ORDER BY NAME, DATETIME DESC
```



