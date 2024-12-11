# Chapter 03 - 다형성과 추상타입

> 객체 지향의 주요 개념(2) - 추상화

## 상속(Inheritance)

> 기존 타입을 그대로 사용하면서 구현을 추가할 수 있도록 해주는 방법

- 보통 private 범위를 갖는 메서드나 필드를 제외한 나머지를 물려받음
- override를 통해 상위 클래스에 정의된 메서드를 새롭게 구현 가능

## 다형성(Polymorphism)

> 한 객체가 여러가지 타입을 갖는 것

- 다형성을 통해 한 객체를 통해 여러 타입에 정의된 기능을 제공할 수 있다.
  - **상속**을 통해 객체에게 다형성을 제공한다.
- 다형성을 구현하는 방법

  - 인터페이스 상속: 타입 정의만을 상속받는 것

    - Java의 경우 Interface를 통해 다형성을 구현한다

    ```java
    interface Animal {
       void makeSound();
    }

    interface Pet extends Animal {
       void play();
    }

    class Dog implements Pet {
       @Override
       public void makeSound() {
           System.out.println("Woof");
       }

       @Override
       public void play() {
           System.out.println("The dog is playing.");
       }
    }

    ```

  - 구현 상속: 클래스 상속을 통해 이루어짐

    - 상위 클래스에 정의된 기능을 재사용하기 위한 목적으로 사용됨
      -> 코드 재사용성을 높임
    - 클래스 간의 관계를 정의하여 계층 구조를 형성.

    ```java
     class Animal {
        void eat() {
            System.out.println("Eating...");
        }
    }

    class Dog extends Animal {
        void bark() {
            System.out.println("Woof");
        }
    }
    ```

## 추상화(Abstraction)

> 데이터나 프로세스 등을 의미가 비슷한 개념이나 표현으로 정의하는 과정

- **클래스에서 추상 타입을 이끌어낸다 != 추상화**
- **콘크리트 클래스**(Concrete class): 상위 타입인 인터페이스에 정의된 기능을 실제로 구현하는 클래스
  - 인터페이스와 콘크리트 클래스는 타입 상속을 통해 연결된다.
- 추상타입의 장점

  - **교체의 유연함**: 추상 타입을 상속받은 콘크리트 클래스는 다형성을 통해 추상 타입으로 동작한다. 이로 인해 상위 수준의 로직을 수정할 필요가 줄어들어 코드의 유연성이 향상됨.
  - **객체의 책임 분리**: 공통된 개념을 도출하여 추상 타입을 정의함으로써, 여러 책임을 가진 객체의 책임을 명확하게 분리할 수 있다. 이를 통해 코드의 가독성과 유지보수성이 높아짐.
  - **테스트 용이성**: 콘크리트 클래스를 사용할 경우 해당 클래스의 구현이 완료되기 전에는 상위 수준의 로직을 테스트할 수 없다. 그러나 인터페이스를 사용하면 콘크리트 클래스를 Mocking하여 생성할 수 있으므로, 해당 클래스의 구현이 완료될 필요 없이 상위 로직을 테스트할 수 있다.

  ```java
  // 인터페이스 정의
  interface Animal {
      void makeSound();
  }

  // 인터페이스 구현
  class Dog implements Animal {
      @Override
      public void makeSound() {
          System.out.println("Woof");
      }
  }

  class Cat implements Animal {
      @Override
      public void makeSound() {
          System.out.println("Meow");
      }
  }

  // 사용 예시
  class AnimalSoundTest {
      void testAnimalSound(Animal animal) {
          animal.makeSound();
      }

      public static void main(String[] args) {
          AnimalSoundTest test = new AnimalSoundTest();
          Animal dog = new Dog();
          Animal cat = new Cat();

          test.testAnimalSound(dog); // 출력: Woof
          test.testAnimalSound(cat); // 출력: Meow
      }
  }

  ```

- 주의 사항
  - 인터페이스는 최초 설계에서 바로 도출되기 보다는, **요구사항이 추가되면서 새롭게 발견된 추상 개념을 통해서 도출된다.**
    - 불필요한 추상화는 프로그램의 복잡도만 증가시킴
  - 인터페이스 명은 사용하는 코드 입장에서 작성해야한다.
