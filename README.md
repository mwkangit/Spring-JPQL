# Spring-JPQL


## Description

본 프로젝트는 JPQL에 대한 정확히 이해 후 상황에 따라 쿼리를 작성하여 성능을 최적화한다. 우선 JPQL에서 허용되는 쿼리의 범위를 확인한 후 Fetch Join, Bulk 등 사용하여 연산의 효율을 증가시킨다. Java 코드로 쿼리를 작성하여 Database 쿼리에 의존하지 않는 로직을 작성할 수 있지만 JPQL도 객체 지향 쿼리이므로 Database 쿼리를 아는 상태에서 JPQL을 사용하는 것이 권장된다. JPQL로 쿼리를 작성 후 실제 Database에 전달되어 실행되는 쿼리를 확인하는 절차를 거쳐야 성능을 최적화 할 수 있으므로 JDBC Template을 로그로 확인할 수 있게 설정해야 한다.



------



## Environment

<img alt="framework" src ="https://img.shields.io/badge/Framework-SpringBoot-green"/><img alt="framework" src ="https://img.shields.io/badge/Language-java-b07219"/> 

Framework: `Spring Boot` 2.6.0

Project: `Maven`

Packaging: `Jar`

IDE: `Intellij`

ORM: `JPA`(Hibernate, Spring Data JPA)

DBMS: `H2-Database`

Dependencies: `Spring Data JPA`



------



## Installation



![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black) 

```
./gradlew build
cd build/lib
java -jar hello-spring-0.0.1-SNAPSHOT.jar
```



![Windows](https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white) 

```
gradlew build
cd build/lib
java -jar hello-spring-0.0.1-SNAPSHOT.jar
```




------



## Core Feature

```java
// Fetch Join

// DISTINCT로 데이터 뻥튀기 제거
String query = "select distinct t from Team t join fetch t.members";
// 데이터 뻥튀기 해결
/* 출력문
team = teamA|members=2
->  member = Member{id=4, username='member1', age=0}
->  member = Member{id=5, username='member2', age=0}
*/

// Bulk

int resultCount = em.createQuery("update Member m set m.age = 20")
                    .executeUpdate();
```

상위 쿼리는 Fetch Join과 DISTINCT를 사용한 것으로 컬렉션 Fetch Join 시 발생하는 중복 엔티티 문제를 해결한다. Fetch Join으로 N + 1 문제를 해결하며 JPA의 DISTINCT는 Database와 다르게 어플리케이션에서 Entity 중복을 해결한다.

하위 쿼리는 Bulk 연산을 이용하여 변경 감지의 많은 연산을 최적화한다. 영속성 컨텍스트를 거치지 않고 바로 쿼리를 반영하는 주의사항이 있지만 실행 시간을 많이 단축할 수 있는 장점이 매우 크서 실무에서 자주 사용한다.



## More Explanation



[Spring-JPQL.md]()
