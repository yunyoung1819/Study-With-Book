# :book: 자바 ORM 프로그래밍 기본편

## :pushpin: 객체지향 쿼리 언어(JPQL)

### 객체지향 쿼리 언어 소개

- JPA는 다양한 쿼리 방법을 지원
  - JPQL
  - JPA Criteria
  - QueryDSL
  - 네이티브 SQL
  - JDBC API 직접 사용, MyBatis, SpringJdbcTemplate 함께 사용
  

### JPQL 소개

- 가장 단순한 조회 방법
  - EntityManager.find()
  - 객체 그래프 탐색(a.getB().getC())
  
- 나이가 18살 이상인 회원을 모두 검색하고 싶다면?


### JPQL 
- JPA를 사용하면 엔티티 객체를 중심으로 개발
- 문제는 검색 쿼리
- 검색을 할 때도 테이블이 아닌 엔티티 객체를 대상으로 검색
- 모든 DB 데이터를 객체로 변환해서 검색하는 것은 불가능
- 애플리케이션이 필요한 데이터만 DB에서 불러오려면 결국 검색 조건이 포함된 SQL이 필요
- JPA는 SQL을 추상화한 JPQL이라는 객체 지향 쿼리 언어 제공
- SQL과 문법 유사, SELECT, FROM, WHERE, GROUP BY, HAVING, JOIN 지원
- JPQL은 엔티티 객체를 대상으로 쿼리
- SQL은 데이터베잇 테이블을 대상으로 쿼리

```
// 검색
String jpql = "select m From Member m where m.name like '%hello%'";

List<Member> result = em.createQuery(jpql, Member.class)
         .getResultList(); 
```