# :book: 자바 ORM 프로그래밍 기본편

## :pushpin: 경로 표현식

### 경로 표현식
- .(점)을 찍어 객체 그래프를 탐색하는 것

```
select m.username -> 상태 필드
    from Member m
        join m.team t   -> 단일 값 연관 필드
        join m.orders o -> 컬렉션 값 연관 필드
where t.name = '팀A'
```

### 경로 표현식 용어 정리
- 상태 필드(state field): 단순히 값을 저장하기 위한 필드 (ex: m.username)
- 연관 필드(association field): 연관관계를 위한 필드
    - 단일 값 연관 필드: @ManyToOne, @OneToOne, 대상이 엔티티(ex: m.team)
    - 컬렉션 값 연관 필드: @OneToMany, @ManyToMany, 대상이 컬렉션(ex: m.orders)
    

### 경로 표현식 특징
- 상태 필드(state field): 경로 탐색의 끝, 탐색 X
- 단일 값 연관 경로: 묵시적 내부 조인(inner join) 발생, 탐색 O
- 컬렉션 값 연관 경로: 묵시적 내부 조인 발생, 탐색 X
    - FROM 절에서 명시적 조인을 통해 별칭을 얻으면 별칭을 통해 탐색 가능