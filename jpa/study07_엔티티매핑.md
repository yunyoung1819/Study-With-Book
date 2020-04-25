# :book: 자바 ORM 프로그래밍 기본편 

## :pushpin: 엔티티 매핑 

> 엔티티 매핑 소개

- 객체와 테이블 매핑: @Entity, @Table
- 필드와 컬럼 매핑: @Column
- 기본 키 매핑: @Id
- 연관관계 매핑: @ManyToOne, @JoinColumn


## 객체와 테이블 매핑

### @Entity

- @Entity가 붙은 클래스는 JPA가 관리, 엔티티라 한다.
- JPA를 사용해서 테이블과 매핑할 클래스는 @Entity 필수
- 주의
    - 기본 생성자 필수(파라미터가 없는 public 또는 protected 생성자)
    - final 클래스, enum, interface, inner 클래스 사용 X
    - 저장할 필드에 final 사용 X


### @Entity 속성 정리

- 속성: name
- JPA에서 사용할 엔티티 이름을 지정한다.
- 기본값: 쿨래스 이름을 그대로 사용 (예: Member)
- 같은 클래스 이름이 없으면 가급적 기본값을 사용한다.


### @Table

- @Table은 엔티티와 매핑할 테이블 지정
- name: 매핑할 테이블 이름 (기본값: 엔티티 이름을 사용)
- catalog: 데이터베이스 catalog 매핑
- schema: 데이터베이스 schema 매핑
- uniqueConstraints(DDL): DDL 생성 시에 유니크 제약 조건 생성


### 데이터베이스 스키마 자동 생성

- DDL을 애플리케이션 실행 시점에 자동 생성
- 테이블 중심 -> 객체 중심
- 데이터베이스 방언을 활용해서 데이터베이스에 맞는 적절한 DDL 생성
- 이렇게 생성된 DDL은 개발 장비에서만 사용
- 생성된 DDL은 운영 서버에서는 사용하지 않거나, 적절히 다듬은 후 사용 


### 데이터베이스 스키마 자동 생성 - 속성

> hibernate.hbm2ddl.auto

옵션 | 설명
----| ----|
create | 기존 테이블 삭제 후 다시 생성 (DROP + CREATE)
create-drop | create와 같으나 종료 시점에 테이블 DROP
update | 변경분만 반영. 추가는 되고 지우는건 X (운영 DB에는 사용하면 안됨)
validate | 엔티티와 테이블이 정상 매핑되었는지만 확인
none | 사용하지 않음 


### 데이터베이스 스키마 자동 생성 - 주의

- **운영 장비에는 절대 create, create-drop, update 사용하면 안된다.**
- 개발 초기 단계는 create 또는 update
- 테스트 서버는 update 또는 validate
- 스테이징과 운영 서버는 validate 또는 none


### DDL 생성 기능 

- 제약조건 추가: 회원 이름은 필수, 10자 초과X
> Column(nullable = false, length = 10)
- 유니크 제약조건 추가
> @Table(uniqueConstraints = {@UniqueConstraint(name = "NAME_AGE_UNIQUE", columnNames={"NAME", "AGE"}})
- DDL 생성 기능은 DDL을 자동 생성할 때만 사용되고 JPA의 실행 로직에는 영향을 주지 않는다.


### 필드와 컬럼 매핑  