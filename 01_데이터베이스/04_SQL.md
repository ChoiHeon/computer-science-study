# SQL

SQL은 RDBMS에서 저장된 데이터와 통신하기 위해 필요한 프로그래밍 언어입니다. 

<br>

## SQL 명령어

SQL 명령어는 크게 4가지로 나눌 수 있습니다.

* DDL (Data Definition Language)

  * 데이터베이스 스키마와 설명을 정의하는 언어입니다.
  * 데이터베이스나 테이블 생성, 변경,삭제 등의 작업이 여기에 포함됩니다.

* DML (Data Management Language)

  * 데이터를 검색, 삽입, 변경, 삭제를 수행할 수 있도록 조작하는 언어입니다.
  * 사용자가 실질적으로 데이터를 조작할 수 있도록 하기 위해 다양한 언어에 DB 기능을 추가한 것입니다.
  * 사용자가 사용하기 쉽도록 자연어에 가깝다는 특징이 있습니다.

* DCL (Data Control Language)

  * 데이터의 무결성, 보안 및 권한 제어 획복 등을 하기 위한 언어입니다.
  * 데이터를 보호하고 관리하는 목적으로 사용됩니다.

  > 무결성(Integrity)는 데이터의 정확성과 일관성을 유지하고 보증하는 것을 가리킵니다.

* TCL (Transaction Contol Language)

  * 트랜젝션을 다루는 언어입니다.

  > 트랜젝션(Transaction)은 데이터베이스에서 데이터를 처리하는 하나의 단위를 의미합니다.

<br>

위의 종류에 따라 명령어를 다음과 같이 분류할 수 있습니다.

| 종류 | 명령어                                                       |
| ---- | ------------------------------------------------------------ |
| DDL  | CREATE, ALTER, DROP, TRUNCATE, COMMENT, RENAME               |
| DML  | SELECT, INSERT, UPDATE, DELETE, MERGE, CALL, EXPLAIN PLAN, LOCK TABLE |
| DCL  | GRANT, REVOKE                                                |
| TCL  | COMMIT, ROLLBACK, SAVEPOINT, SET TRANSACTION                 |

