# CRUD

CRUD는 컴퓨터 프로그램이 데이터를 처리하기 위해 필요한 기능들입니다.

> Create    : 새로운 정보를 생성
>
> Read	   : 처리할 정보를 조회
>
> Update   : 기존의 정보를 갱신
>
> Delete	 : 기존의 정보를 삭제

각각 생성, 조회, 갱신, 삭제를 의미합니다.

<br>

뿐만 아니라 데이터베이스의 데이터를 다루는 데 사용하는 SQL문과 대응됩니다.

| 기능명 | SQL 명령어 |
| ------ | ---------- |
| CREATE | INSERT     |
| READ   | SELECT     |
| UPDATE | UPDATE     |
| DELETE | DELETE     |

<br>

## 사용 방법

1. 테이블 생성

   > CREATE TABLE [테이블 명] (속성1, 속성2, ...);

   테이블의 이름과 테이블의 가리리는 객체가 가지는 속성들을 정의합니다.

<BR>

1. 데이터 추가

   > INSERT INTO [테이블 명] (속성1, 속성2, ...) VALUES (데이터1, 데이터2, ...);

   데이터를 넣을 테이블의 이름과 테이블이 가지는 속성들을 표시합니다.
   순차적으로 데이터들이 속성의 값으로 적용됩니다.

<br>

3. 데이터 조회

   > SELECT [속성 명] FROM  [테이블 명];

   조회할 테이블의 이름과 속성의 이름을 통해 원하는 데이터를 조회합니다.

<br>

4. 데이터 갱신

   > UPDATE [테이블 명] SET 속성1 = 데이터1 WHERE 속성2 = 데이터2;

   테이블에서 데이터2와 일치하는 속성2 값을 가진 튜플에 대해 속성1의 값을 데이터1로 변경합니다.

<BR>

5. 데이터 삭제

   > DELETE FROM [테이블 명] WHERE 조건;

   조건에 맞는 튜플을 삭제합니다.