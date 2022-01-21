# :book: 스프링 핵심 원리

## :pushpin: 다양한 의존관계 주입 방법

### 다양한 의존관계 주입 방법
- 의존관계 주입은 크게 4가지 방법이 있다.
    - 생성자 주입
    - 수정자 주입(setter 주입)
    - 필드 주입
    - 일반 메서드 주입
    
### 생성자 주입 
- 이름 그대로 생성자를 통해서 의존 관계를 주입받는 방법이다.
- 특징
    - 생성자 호출 시점에 딱 1번만 호출되는 것이 보장된다.
    - 불변, 필수 의존관계에 사용
    
```java
@Component
public class OrderServiceImpl implements OrderService {
    
    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;
    
    @Autowired
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
}
```

- **중요! 생성자가 딱 1개만 있으면 @Autowired를 생략해도 자동 주입된다. 물론 스프링 빈에만 해당된다.**

```java
@Component
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;
    
    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
}
```

### 수정자 주입(setter 주입)
- setter라 불리는 필드의 값을 변경하는 수정자 메서드를 통해서 의존관계를 주입하는 방법이다.
- 특징
  - 선택, 변경 가능성이 있는 의존관계에 사용
  - 자바빈 프로퍼티 규역의 수정자 메서드 방식을 사용하는 방법이다.
  
```java
@Component
public class OrderServiceImpl implements OrderService {
    
    private MemberRepository memberRepository;
    private DiscountPolicy discountPolicy;
    
    @Autowired
    public void setMemberRepository(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    
    @Autowired
    public void setDiscountPolicy(DiscountPolicy discountPolicy) {
        this.discountPolicy = discountPolicy;
    }
}
```

> 참고: @Autowired의 기본 동작은 주입할 대상이 없으면 오류가 발생한다. 주입할 대상이 없어도 동작하게 하려면
> @Autowired(required = false)로 지정하면 된다.

> 참고: 자바빈 프로퍼티, 자바에서는 과거부터 필드의 값을 직접 변경하지 않고, setXxx, getXxx 라는 메서드를 통해서
> 값을 읽거나 수정하는 규칙을 만들었는데, 그것이 자바빈 프로퍼티 규역이다.

### 자바빈 프로퍼티 규약 예시
```java
class Data {
    private int age;
    public void setAge(int age) {
        this.age = age;
    }
    
    public int getAge() {
        return age;
    }
}
```

### 필드 주입
- 이름 그대로 필드에 바로 주입하는 방법이다.
- 특징
  - 코드가 간결해서 많은 개발자들을 유혹하지만 외부에서 변경이 불가능해서 테스트하기 힘들다는 치명적인 단점이 있다.
  - DI 프레임워크가 없으면 아무것도 할 수 없다.
  - 사용하지 말자!
    - 애플리케이션의 실제 코드와 관계없는 테스트 코드
    - 스프링 설정을 목적으로 하는 @Configuration 같은 곳에서만 특별한 용도로 사용
  
```java
@Component
public class OrderServiceImpl implements OrderService {
    
    @Autowired
    private MemberRepository memberRepository;
    
    @Autowired
    private DiscountPolicy discountPolicy;
}
```

### 일반 메서드 주입
- 일반 메서드를 통해서 주입 받을 수 있다.
- 특징 
  - 한번에 여러 필드를 주입 받을 수 있다.
  - 일반적으로 잘 사용하지 않는다.
  
```java
@Component 
public class OrderServiceImpl implements OrderService {
    
    private MemberRepository memberRepository;
    private DiscountPolicy discountPolicy;
    
    @Autowired
    public void init(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
}
```

> 참고: 어쩌면 당연한 이야기지만 의존관계 자동 주입은 스프링 컨테이너가 관리하는 스프링 빈이어야 동작한다.
> 스프링 빈이 아닌 `Member` 같은 클래스에서 @Autowired 코드를 적용해도 아무 기능도 동작하지 않는다.


### 옵션 처리
- 주입할 스프링 빈이 없어도 동작해야할 때가 있다.
- 그런데 @Autowired만 사용하면 required 옵션의 기본값이 true로 되어있어서 자동 주입 대상이 없으면 오류가 발생한다.
- 자동 주입 대상을 옵션으로 처리하는 방법은 다음과 같다.
  - @Autowired(required=false): 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출안됨
  - org.springframework.lang.@Nullable: 자동 주입할 대상이 없으면 null이 입력된다.
  - Optional<>: 자동 주입할 대상이 없으면 Optional.empty가 입력된다.
  

```
// 호출안됨
@Autowired(required=false)
public void setNoBean1(Member member) {
    System.out.println("setNoBean1 = " + member);
}

// null 호출
@Autowired
public void setNoBean2(@Nullable Member member) {
  System.out.println("setNoBean2 = " + member);
}

// Optional.empty 호출
@Autowired(required=false)
public void setNoBean3(Optional<Member> member) {
  System.out.println("setNoBean3 = " + member);
}
```

- Member는 스프링 빈이 아니다.
- setNoBean1()은 @Autowired(required = false)이므로 호출 자체가 안된다.

- 출력 결과
```
setNoBean2 = null
setNoBean3 = Optional.empty
```

> 참고: @Nullable, Optional은 스프링 전반에 걸쳐서 지원된다. 예를 들어서 생성자 자동 주입해서 특정 필드에만 사용해도 된다.

