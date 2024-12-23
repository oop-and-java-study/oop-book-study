### **Chapter 1. 들어가기**
<br>

**◼ 소프트웨어의 가치**
+ 사용자가 요구하는 기능을 올바르게 제공해야 함
+ 시간이 지남에 따라 변경하는 요구사항에 따라 유연하게 변화할 수 있어야 함

즉, 개발자는 가치있는 소프트웨어를 사용자에게 제공하기 위해, 유연한 구조를 만들어주는 핵심 기법 중의 하나인 객체 지향에 대해 이해하고 상황에 따라 알맞은 디자인 패턴을 적용할 수 있어야 한다.

<br>

---
### **Chapter 2. 객체지향**
<br>

#### **1. 절차지향(Procedural Oriented)**

번역에 따라 절차지향이란 의미가 순서에 따라 프로그래밍하는 것으로 생각할 수 있으나, 명확하게는 프로시저(procedure)를 이용한 프로그래밍 기법을 의미한다.

**◼ 절차지향 프로그래밍의 특징**
+ 데이터를 사용하여 기능을 구현하며, 여러 프로시저가 동일한 데이터를 공유
+ 데이터와 함수를 별도로 분리하여 관리
+ 구현이 단순하고 실행속도가 빨라 작은 프로그램 작성에 유리

작은 단위의 알고리즘 또는 기계에 들어가는 단순한 프로그램의 경우, 절차지향적으로 구현하는 것이 성능 측면에서는 유리할 수 있다. 하지만 기능과 요구사항이 복잡한 대규모 프로그램에는 적합하지 않다.

**◼ 절차지향 프로그래미의 단점**
+ 데이터 수정 시, 여러개의 프로시저를 모두 수정해야 함
+ 같은 데이터를 프로시저마다 다른 의미로 사용하는 경우가 발생

따라서 절차지향은 단순하고 컴퓨터의 처리흐름과 비슷한 구조라 이해가 쉽지만, 변경에 유연하지 않기 때문에 일반적으로 사용하는 B2C 웹/앱 서비스에는 적합하지 않다.

<br>

---

#### **2. 객체지향(Object Oriented)**

데이터와 함수가 분리된 절차지향과 다르게 객체지향은 데이터와 함수가 하나의 객체(Object)로서 관리되어, 다른 객체의 데이터에 대해서는 함수로 제공해주지 않는 이상 접근할 수 없다. 절차지향과는 반대로 작은 프로젝트 규모에서는 오히려 복잡한 구조로 이해하기 힘들고 성능이 떨어질 수 있다. 하지만 소프트웨어이 생명주기가 길어질수록 변경에 유연하다는 것이 가장 큰 장점이다.

<br>

**◼ 객체란?**

데이터 중심인 프로시저와 반대로 객체는 **기능(Operation) 중심**이다. 객체 내부의 데이터가 어떻게 구성되어있고 변경되는지 보다 어떤 기능을 제공하는지가 더 중요하다.

간단 용어 정리
+ 인터페이스 : 객체가 제공하는 기능에 대한 명세서
+ 클래스 : 인터페이스에 정의된 기능의 구현과 관련 데이터 정의서
+ 메세지 : 다른 객체에서 기능 실행을 위해 요청을 보내는 것 _(ex. 메서드 호출)_
 
<br>

**◼ 객체의 책임과 크기 설정**

설계의 가장 첫 번째는 객체를 어떻게 구성하여 각 객체에 어떤 책임을 할당할지 결정하는 것이다. 이 때, 객체가 갖는 책임이 작으면 작을수록 좋다. 객체의 책임이 크다는 것은 한 객체에서 기능이 많다는 것이고, 기능이 많아질수록 절차지향적인 구조를 갖게되어 기능 변경의 어려움이 발생하기 때문이다. _(Part2의 SOLID 원칙 참고)_

<br>

**◼ 의존**

한 객체에서 다른 객체를 생성하거나 메서드를 호출할 때 발생한다. 즉 의존하는 객체가 변경이 생길 때, 영향을 받는다는 의미이다.

+ 기존 코드
  
    ```java
    public class FileDataReader {
        void setFile(File file) {
            // 기능
        }
    }

    public class FlowController {
        public void process() {
            // FileDataReader를 의존
            FileDataReader fdr = new FileDataReader();
            File file = new File("/test/text.txt");
            fdr.setFile(file);
        }
    }
    ```

+ FileDataReader 변경 발생 시
  
    ```java
    public class FileDataReader {
        // File 객체 대신 파일 경로를 문자열로 받도록 수정
        void setFile(String path) {
            // 기능
        }
    }

    public class FlowController {
        public void process() {
            FileDataReader fdr = new FileDataReader();
            // 필수적으로 메서드를 호출하는 부분에 변경이 발생
            fdr.setFile("/test/text.txt");
        }
    }    
    ```

변경은 의존 관계를 따라 영향이 전이 된다. 따라서, 프로그램을 작성할 때 의존 순환이 발생하지 않도록 주의해야 한다. 의존 순환 상태가 되면, 객체에서 발생된 변경사항이 해당 객체를 의존하던 객체를 변경시키고, 마지막에는 다시 처음 변경이 된 객체까지 다시 변경이 발생할 수 있다.

`class A --> class B --> class C --> classA`

C클래스의 변경으로 B,A까지 변경되고, A 변경사항으로 인해 다시 C클래스가 변경될 수 있음

<br>

**◼ 캡슐화**

캡슐화란 기능이 내부적으로 어떻게 구현되었는지 숨기는 것. 캡술화가 잘 된 코드는 기능에 변경이 있어도 이를 의존하는 다른 객체들을 변경할 필요가 없다. 즉, 캡슐화를 잘할수록 변경에 따른 영향이 작아 프로그램 수정이 용이하다.

+ 캡슐화가 안된 케이스
    
    만료 정책에 변경이 있을 때마다, 만료 프로세스가 구현된 모든 클래스가 수정되어야 함
    ```java
    @Getter
    public class Member {
        private Date expireDate;
        private boolean male;
    }

    // 각각의 객체에서 만료여부 확인
    public class A {
        void process(Member member) {
            if(member.getExpireDate != null && member.getExpiredDate.getDate() < System.currentTimeMillis()) {
                throw new ExpiredMemberException();
            }
            // 통과 시, 프로세스 정상수행
        }
    }

    public class B {
        void process(Member member) {
            if(member.getExpireDate != null && member.getExpiredDate.getDate() < System.currentTimeMillis()) {
                throw new ExpiredMemberException();
            }
            // 통과 시, 프로세스 정상수행
        }
    }
    ```
+ 캡슐화가 잘된 케이스
  
  Member 클래스 한 곳만 수정해도 모두 적용됨
  ```java
    public class Member {
        private Date expireDate;
        private boolean male;

        public boolean isExpired() {
            // 1차 요구사항
            /* 
            return expireDate != null && expireDate.getDate() < System.currentTimeMillis(); 
            */

            // 변경된 요구사항
            if(expireDate == null) return true;

            if(male) {
                return expireDate.getDate() < System.currentTimeMillis();
            } else {
                return expireDate.getDate() < System.currentTimeMillis() - DAY30;
            }
        }
    }

    // A,B는 만료정책이 바뀌어도 변경 없음
    public class A {
        void process(Member member) {
            if(member.isExpired()) {
                throw new ExpiredMemberException();
            }
            // 통과 시, 프로세스 정상수행
        }
    }

    public class B {
        void process(Member member) {
            if(member.isExpired()) {
                throw new ExpiredMemberException();
            }
            // 통과 시, 프로세스 정상수행
        }
    }
  ```

**캡슐화를 잘하기 위한 Tip**

두 개의 규칙을 지켜라!
+ Tell, Don't Ask : 데이터는 모르겠고, 그냥 기능 실행해줘!
+ 데미테르의 법칙(Law of Demeter) : 메서드의 메서드를 호출하지 마라!
```java
member.isExpired(); // 만료일을 묻지 않고 만료여부를 알려달라는 기능 실행 요청, Tell Don't Ask!

member.getExpiredDate().getDate(); // 데미테르 법칙 위반
meber.someMethod(); // 데미테르 법칙! 자연스럽게 캡술화 성공
```

<br>

---

#### **정리**

객체지향 설계의 기초
+ 객체에 대한 정의, 책임, 의존 설정
+ 캡슐화를 통한 내부 구현의 유연성 확보
+ [추상화를 통한 외부 구현의 유연성 확보](/part1/chapter03/dayoung.md)