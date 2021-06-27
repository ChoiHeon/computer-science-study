# 조인

조인은 서로 다른 두 개 이상의 테이블에서 연관된 속성을 조건으로 결합하는 방법입니다.

<br>

![](https://t1.daumcdn.net/cfile/tistory/99219C345BE91A7E32)

조인 종류

* INNER 조인 (기본적인 조인을 의미)
* LEFT , RIGHT, OUER 조인
* CROSS(CARESIAN) 조인 

<br>

### 1. INNER 조인

다음과 같은 방식으로 사용이 가능합니다.

> SELECT 테이블1.속성, 테이블2.속성
>
> FROM 테이블1
>
> JOIN 테이블2
>
> ON 테이블1.속성 = 테이블2.속성

ON 대신 WHERE의 사용이 가능하며 결과적으로 교집합을 구할 수 있습니다.

또는 아래의 방법으로도 가능합니다.

> SELECT 테이블1.속성, 테이블2.속성
>
> FROM 테이블1, 테이블2
>
> WHERE 테이블1.속성 = 테이블2.속성

<BR>

### 2. LEFT OUTER 조인, RIGHT OUTER 조인

테이블 A, B가 주어질 때 LEFT OUTER 조인은 (A - B) u (A n B), RIGHT OUTER 조인은 (B - A) u (A n B)를 구할 수 있습니다. 
다음은 각각 LEFT OUTER 조인, RIGHT OUTER 조인의 사용방법입니다.

> SELECT 테이블1.속성, 테이블2.속성
>
> FROM 테이블1
>
> LEFT JOIN 테이블2
>
> ON 테이블1.속성 = 테이블2.속성
>
> (WHERE 테이블2.속성 IS NULL)

<br>

> SELECT 테이블1.속성, 테이블2.속성
>
> FROM 테이블1
>
> RIGHT JOIN 테이블2
>
> ON 테이블1.속성 = 테이블2.속성
>
> (WHERE 테이블1.속성 IS NULL)

두 방식 모두 마지막 줄을 추가하면 두 테이블의 교집합을 제외합니다.

<br>

### 3. FULL OUTER 조인

테이블 A, B가 주어질 때 FULL OUTER 조인은 (A - B) u (B - A) u (A n B)을 구할 수 있습니다.

> SELECT 테이블1.속성, 테이블2.속성
>
> FROM 테이블1
>
> FULL OUTER  JOIN 테이블2
>
> ON 테이블1.속성 = 테이블2.속성
>
> (WHERE 테이블1.속성 IS NULL OR 테이블2.속성 IS NULL)

마지막 줄을 추가하면 두 테이블의 교집합을 제외할 수 있습니다.

<br>

### 4. CROSS(CARESIAN) 조인 

CROSS 조인은 각 테이블의 속성들을 결합합니다. 만약 테이블 A의 튜플과 속성 개수가 X, Y이고 테이블 B가 P, Q개라면, 두 테이블의 CROSS 조인의 결과 속성의 수는 (Y + Q)이고 튜플의 개수는 (X x P) 입니다.

> SELECT 테이블1.속성, 테이블2.속성
>
> FROM 테이블1
>
> CROSS JOIN 테이블2

<br>

### Example

문제: https://programmers.co.kr/learn/courses/30/lessons/59042

```sql
SELECT o.ANIMAL_ID, o.NAME
FROM ANIMAL_OUTS AS o
LEFT JOIN ANIMAL_INS AS i
ON o.ANIMAL_ID = i.ANIMAL_ID
WHERE i.ANIMAL_ID IS NULL
ORDER BY o.ANIMAL_ID
```

* LEFT OUTER 조인에서 두 테이블의 공통된 데이터를 제거하는 SQL 코드입니다.

<br>

문제: https://programmers.co.kr/learn/courses/30/lessons/59043

```sql
SELECT i.ANIMAL_ID, i.NAME
FROM ANIMAL_INS AS i
JOIN ANIMAL_OUTS AS o
ON i.ANIMAL_ID = o.ANIMAL_ID
WHERE i.DATETIME > o.DATETIME
ORDER BY i.DATETIME
```

* DATETIME 자료형은 부등호 연산이 가능합니다. 큰 DATETIME이 더 미래의 날짜를 의미합니다.

<br>

문제: https://programmers.co.kr/learn/courses/30/lessons/59044

```sql
SELECT i.NAME, i.DATETIME
FROM ANIMAL_INS AS i
LEFT JOIN ANIMAL_OUTS AS o
ON i.ANIMAL_ID = o.ANIMAL_ID
WHERE o.ANIMAL_ID IS NULL
ORDER BY i.DATETIME
LIMIT 3
```

<BR>

문제: https://programmers.co.kr/learn/courses/30/lessons/59045

```sql
SELECT i.ANIMAL_ID, i.ANIMAL_TYPE, i.NAME
FROM ANIMAL_INS AS i
JOIN ANIMAL_OUTS AS o
ON i.ANIMAL_ID = o.ANIMAL_ID
WHERE i.SEX_UPON_INTAKE = "Intact Female" AND o.SEX_UPON_OUTCOME = "Spayed Female" OR i.SEX_UPON_INTAKE = "Intact Male" AND o.SEX_UPON_OUTCOME = "Neutered Male"
ORDER BY i.ANIMAL_ID
```



