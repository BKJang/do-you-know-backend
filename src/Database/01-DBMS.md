# DBMS

우선 `DBMS`를 알기 이전에 `Database`에 대해서 한 번 짚고 넘어갈 필요가 있다.

### Database?

- 데이터의 집합 (a Set of Data)이다.
- 여러 애플리케이션들의 통합된 정보들을 저장하여 운영할 수 있는 공용(share) 데이터의 집합이다.

Database는 데이터의 집합이기 때문에 효율적으로 저장(C), 검색(R), 갱신(U), 삭제(D)할 수 있도록 데이터 집합들끼리 연관시키고 조직화되어야 한다.

#### 특성

- 실시간 접근성(Real-time Accessability)
  - 사용자의 요구를 즉시 처리할 수 있다.
- 계속적인 변화(Continuous Evolution)
  - 정확한 값을 유지하려고 삽입·삭제·수정 작업 등을 이용해 데이터를 지속적으로 갱신할 수 있다.
- 동시 공유성(Concurrent Sharing)
  - 사용자마다 서로 다른 목적으로 사용하므로 동시에 여러 사람이 동일한 데이터에 접근하고 이용할 수 있다.
- 내용 참조(Content Reference)
  - 저장한 데이터 레코드의 위치나 주소가 아닌 사용자가 요구하는 데이터의 내용, 즉 데이터 값에 따라 참조할 수 있어야 한다.


### DBMS(Database Management System)

`DBMS`란 용어에서 알 수 있듯이 데이터베이스를 관리하는 시스템 즉, 소프트웨어다.
`DBMS`를 통해 우리는 위에서 살펴보았던 데이터베이스의 특성들을 보다 쉽고 효율적으로 보장할 수 있다.

`DBMS`의 필수 기능에는 3가지가 있는데 `정의`, `조작`, `제어`가 있다.

- 정의 : 데이터 베이스의 논리적, 물리적 구조를 정의하는 기능.
- 조작 : 데이터를 검색, 삭제, 갱신, 삽입, 삭제하는 기능.
- 제어 : 데이터베이스의 내용 정확성과 안전성을 유지하도록 제어하는 기능.

#### 장/단점

- 장점
  - 데이터 중복이 최소화
  - 데이터의 일관성 및 무결성 유지
  - 데이터 보안 보장

- 단점 
  - 비싼 운영비
  - 백업 및 복구에 대한 관리가 복잡
  - 부분적 데이터베이스 손실이 전체 시스템을 정지

위의 단점들이 있는 것이 사실이긴 하지만, 사실상 Database를 사용할 때 `DBMS`를 사용하지 않는 경우는 거의 없다고 보는게 맞을 것 같다.
대표적인 `DBMS`의 종류에는 Oracle, SQL Server, MySQL, DB2 등이 있다.

### MySQL 설치 및 실행(Mac OS 기준)

- 설치

```sh
brew install mysql
```

- 실행 및 정지

```sh
mysql.server start
mysql.server stop
mysql.server restart
```

- 데몬으로 실행 및 정지
  - 운영체제의 백그라운드로 MySQL이 계속 실행되도록 하고 싶다면 HomeBrew가 제공하는 명령을 이용하면 된다.

```sh
brew services start mysql
brew services stop mysql
brew services restart mysql
brew services list
```

---

#### 🙏 Reference

- [edwidth - 부스트코스 웹 백엔드](https://www.edwith.org/boostcourse-web-be/lecture/58930/)
