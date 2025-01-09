# chapter 05

### 객체 지향의 설계 원칙 - SOLID 설계 원칙
> - 단일 책임 원칙 (SRP) Single Responsibility principle
> - 개방-폐쇄 원칙 (OCP) Open-Closed Principle
> - 리스코프 치환 원칙 (LSP) Liskov Substitution Principle
> - 인터페이스 분리 원칙 (ISP) Interface Segregation Principle
> - 의존 역전 원칙 (DIP) Dependency Inversion Principle

---
#### 1. 단일 책임 원칙
> 클래스는 단 한개의 책임을 가져야 한다

단일 책임 원칙이 지켜지지 않으면 다른 원칙을 잘 지키려해도 효과가 떨어지므로 가장 중요한 원칙이다.

- Q. 한 개의 책임이란?
  - 프로그램 내에서 여러 이유로 변경 혹은 확장이 일어날 수 있는데, 한 개의 책임은 하나의 이유로 발생하는 소스의 변화라고 볼 수 있음
  - 따라서 서로 다른 이유로 바뀌는 책임들이 한 클래스에 포함되어 있다면 이 클래스는 단일 책임 원칙을 어기고 있다고 볼 수 있음
  - 메서드를 실행하는 주체를 확인하였을 때, 다른 곳에서 메서드를 실행한다면 다른 책임을 지닐 가능성이 높음

- Q. 단일 책임 원칙을 지키지 않는다면?
  - 소스 수정 발생 시 연쇄적인 변화를 요구함 > 코드를 절차 지향적으로 만들어 변경을 어렵게 함
  - 소스의 재사용을 어렵게 함

- Q. 두 개의 책임을 지닌 클래스를 단일 책임 원칙을 지키도록 하기 위해선? 
  - 각 책임을 지는 클래스 두 개로 분리
  - 두 클래스 간에 주고받을 데이터 타입을 추상화하여 각 클래스에서 필요한 책임에 대한 기능만 수행

---
#### 2. 개방-폐쇄 원칙
> 확장에는 열려 있어야 하고, 변경에는 닫혀 있어야 한다
>  > 기능을 변경하거나 확장할 수 있으면서,<br>
>  > 그 기능을 사용하는 코드는 수정하지 않는다

개방-폐쇄 원칙은 프로그램의 유연성을 위해 지켜져야 하는 원칙이다.<br>
프로그램 내에서 변화가 예상되는 부분을 추상화해서 변경의 유연함을 얻도록 해준다.

- 구현 방법
  - 변경되는 부분을 추상화하여 변경에 대응을 용이하게 함
  - 상속을 이용하여 구현 (EX. 템플릿 패턴)

- Q. 개방-폐쇄 원칙을 지키지 않는다면?
  - 소스 내에 다운 캐스팅이 일어남 (instanceof 등으로 타입을 확인하여 특정 타입마다 다운캐스팅을 진행해야하는 상황 발생)
  - 비슷한 if-else 문의 반복이 잦음 (위 상황 처럼 특정 타입마다 다운캐스팅 혹은 다른 로직의 수행이 발생)
  
---
#### 3. 리스코프 치환 원칙
> 상위 타입의 객체를 하위 타입의 객체로 치환해도 상위 타입을 사용하는 프로그램은 정상적으로 동작해야 한다

리스코프 치환 원칙은 다형성에 관한 원칙을 제공하며, 기능의 명세(또는 계약)와 확장에 대한 내용이다<br>
리스코프 치환 원칙이 지켜지지 않으면 다형성에 기반한 개방-폐쇄 원칙 또한 지켜지기 어렵다.

- Q. 리스코프 치환 원칙을 지키지 않는다면?
  - instanceof 연산자를 자주 사용하게 됨 (이는 리스코프 치환 원칙을 위배했을 때 주로 발생, 개방-폐쇄 원칙과도 연관)
    - 개념적으로 상속 관계에 있어도, 제공 기능에 따라 클래스의 상속 관계를 위반하는 경우 (EX. 직사각형-정사각형) <br>
      이를 상속관계로 구현 했을 때, 상위 타입(직사각형)에서 제공하는 기능이 하위 타입(정사각형)에선 적용되지 않는 경우가 발생. <br>
      위 문제의 해결을 위해 instanceof 연사자 등으로 특정 하위 클래스에 대한 예외처리를 하게 함 
  - 하위 타입 구현 시, **상위 타입에서 정의한 명세를 벗어나서 구현**하게 되는 경우, 하위클래스 구현체 인스턴스가 할당된 상위 클래스 메소드에서 비정상적인 동작을 할 수 있다.
    - 상위 타입에서 정의한 명세를 벗어나서 구현의 사례
      > - 명시된 명세에서 벗어난 값을 리턴한다
      > - 명시된 명세에서 벗어난 익셉션을 발생한다 
      > - 명세된 명세에서 벗어난 기능을 수행한다
  
---
#### 4. 인터페이스 분리 원칙
> 인터페이스는 그 인터페이스를 사용하는 클라이언트를 기준으로 분리해야 한다 (클라이언트는 자신이 사용하는 메소드에만 의존해야한다)

클라이언트 입장에서 인터페이스를 분리하는 원칙으로, <br>
클라이언트로부터 발생하는 인터페이스 변경의 여파가 다른 클라이언트에 미치는 영향을 최소화하게 해준다
<br><br>
단일 책임 원칙과 같이, 잘 지켜진 인터페이스 분리 원칙은 인터페이스와 콘크리트 클래스(구현체)의 재사용성을 높여주는 효과를 지님

---
#### 5. 의존 역전 원칙
> 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안 된다. 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야 한다.

[ 용어 정의 ]
- **고수준 모듈** : 어떤 의미 있는 단일 기능을 제공하는 모듈, 즉 프로그램 기능의 정책이 담긴 흐름 등이 이에 해당한다.
- **저수준 모듈** : 고수준 모듈의 기능을 구현하기 위해 필요한 하위 기능의 실제 구현, 기능을 위한 상세 내역으로 프로그램 운영 시 고수준 모듈보다 변경이 발생할 가능성이 높다

의존 역전 원칙은 **추상화**를 통하여 변경에 유연한 코드를 작성할 수 있게 도와주며<br>
리스코프 치환 원칙과 함께 개방-폐쇄 원칙을 따르는 설계를 만들어주는 기반이 됨<br>

- Q. 의존 역전 원칙을 지키지 않는다면?
  - 고수준 모듈이 저수준 모듈의 구현에 의존한다면, 비교적 변경 혹은 확장이 잦은 저수준 모듈이 변경될 때, 고수준 모듈이 함께 변경되는 상황이 발생하기 쉬움
  - 의존 역전 원칙이 잘 지켜지면 저수준 모듈이 변경되더라고 고수준 모듈은 변경되지 않는다!

- 의존 역전 원칙은 소스 코드의 의존를 역전 시키는 것이지, 런타임에서의 의존 관계를 역전시키는 것은 아님.
  - 런타임의 의존은 고수준 모듈 -> 저수준 모듈로 향한다.
  - 소스 코드의 의존은 저수준 모듈이 고수준 모듈에서 생성하는 추상화 된 클래스를 의존하는 역전이 일어남.
  - 런타임에서의 의존과 소스코드의 의존은 구분되어야 함

---

### 정리
1. 단일 책임 원칙 & 인터페이스 분리 원칙
   - 객체가 커지는 것을 방지하여 한 기능의 변경이 다른 부분에 미치는 영향을 최소화 해준다
   - > 다른 부분에 미치는 영향을 최소화 한다는 의미가 캡슐화
   
2. 개방-폐쇄 원칙 ( & 리스코프 치환 원칙 & 의존 역전 원칙 )
   - 변화되는 부분을 추상화하고 다형성을 이용하여 기능을 확장할 때 기존 코드를 수정하지 않도록 해준다.