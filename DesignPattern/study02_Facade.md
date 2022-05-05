# :book: 디자인 패턴

## :pushpin: Facade pattern

### facade pattern
- Facade는 건물의 앞쪽 정면이라는 뜻을 가진다. 여러 개의 객체와 실제 사용하는 서브 객체의 사이에 복잡한 의존관계가 있을 때,
중간에 facade 라는 객체를 두고, 여기서 제공하는 interface만을 활용하여 기능을 사용하는 방식이다.
- facade는 자신이 가지고 있는 각 클래스의 기능을 명확히 알아야 한다.

![](./images/facade.png)