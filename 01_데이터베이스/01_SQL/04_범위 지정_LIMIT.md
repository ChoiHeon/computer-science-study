# 행 범위 지정

테이블에서 연속된 튜플의 일부를 얻을 수 있습니다.

> LIMIT 3  		--> 위에서 1번째 ~ 3번째 튜플
>
> LIMIT 2, 5 	 --> 위에서 2번째 ~ 5번째 튜플

<BR>

## Example

문제 출처: https://programmers.co.kr/learn/courses/30/lessons/59405

```SQL
SELECT NAME
FROM ANIMAL_INS
ORDER BY DATETIME
LIMIT 1
```

