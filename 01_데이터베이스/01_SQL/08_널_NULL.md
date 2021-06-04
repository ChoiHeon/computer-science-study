# 널

값이 없음을 의미하는 널 (NULL)을 데이터의 값으로 허용할 수 있습니다.

허용된 속성에 대해 다음과 같은 방법으로 확인할 수 있습니다.

> WHERE (NOT) 속성명 IS NULL

속셩명 앞에 NOT을 추가하면 NULL이 아닌지를 알 수 있습니다.

<br>

널 값을 다르게 표시하고 싶을 경우 IFNULL 함수를 사용하면 됩니다.

> IFNULL(속성명, 대체 값)

속성 값이 NULL일 경우 대체 값으로 표시합니다.

<br>

# Example

문제: https://programmers.co.kr/learn/courses/30/lessons/59407

``` sql
SELECT ANIMAL_ID
FROM ANIMAL_INS
WHERE NOT NAME IS NULL
ORDER BY ANIMAL_ID
```

<br>

문제: https://programmers.co.kr/learn/courses/30/lessons/59410

```sql
SELECT ANIMAL_TYPE, IFNULL(NAME, "No name"), SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID
```

