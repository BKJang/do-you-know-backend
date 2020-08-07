# SQL

SQL은 데이터를 보다 쉽게 검색하고 추가, 삭제, 수정 같은 조작을 할 수 있도록 고안된 컴퓨터 언어이며 관계형 데이터베이스에서 데이터를 조작하고 쿼리하는 표준 수단이다.

### DML(Data Manipulation Language)

- **데이터를 조작**하기 위해 사용한다.
- INSERT, UPDATE, DELETE, SELECT

#### SELECT

```sql
select empno, name, job from employee order by name;
```

#### INSERT

```sql
insert into 테이블명(필드1, 필드2, 필드3, 필드4, … ) 
values ( 필드1의 값, 필드2의 값, 필드3의 값, 필드4의 값, … )
```

```sql
insert into ROLE (role_id, description)
values ( 200, 'CEO');
```

#### UPDATE

```sql
update  테이블명
set  필드1=필드1의값, 필드2=필드2의값, 필드3=필드3의값, …
where 조건식
```

```sql
update ROLE
set description = 'CTO'
where role_id = 200;
```

#### DELETE

```sql
delete
from  테이블명
where  조건식
```

```sql
delete
from ROLE
where role_id = 200;
```

### DDL(Data Definition Language)

- 데이터베이스의 **스키마를 정의하거나 조작**하기 위해 사용
- CREATE, DROP, ALTER

#### CREATE

```sql
create table 테이블명( 
  필드명1 타입 [NULL | NOT NULL][DEFAULT ][AUTO_INCREMENT], 
  필드명2 타입 [NULL | NOT NULL][DEFAULT ][AUTO_INCREMENT], 
  필드명3 타입 [NULL | NOT NULL][DEFAULT ][AUTO_INCREMENT], 
  ........... 
  PRIMARY KEY(필드명) 
);
```

```sql
create table EMPLOYEE2(   
  empno      INTEGER NOT NULL PRIMARY KEY,  
  name       VARCHAR(10),   
  job        VARCHAR(9),   
  boss       INTEGER,   
  hiredate   VARCHAR(12),   
  salary     DECIMAL(7, 2),   
  comm       DECIMAL(7, 2),   
  deptno     INTEGER
);
```

#### ALTER

```sql
alter table 테이블명
add  필드명 타입 [NULL | NOT NULL][DEFAULT ][AUTO_INCREMENT];
```

```sql
alter table EMPLOYEE2
add birthdate varchar(12);
```

#### DROP

```sql
drop table 테이블이름;
```

```sql
drop table EMPLOYEE2;
```

### DCL(Data Control Language)

- **데이터를 제어**하기 위해 사용한다.
- 권한을 관리하고, 테이터의 보안, 무결성 등을 정의한다.
- GRANT, REVOKE

#### 사용자 생성 및 권한 부여

- db이름 뒤의 * 는 모든 권한을 의미한다.
- @’%’는 어떤 클라이언트에서든 접근 가능하다는 의미이고, @’localhost’는 해당 컴퓨터에서만 접근 가능하다는 의미다.
- flush privileges는 DBMS에게 적용을 하라는 의미다.

```sql
grant all privileges on db이름.* to 계정이름@'%' identified by ＇암호’;
grant all privileges on db이름.* to 계정이름@'localhost' identified by ＇암호’;
flush privileges;
```

---

#### 🙏 Reference

- [edwidth - 부스트코스 웹 백엔드](https://www.edwith.org/boostcourse-web-be/lecture/58934/)
