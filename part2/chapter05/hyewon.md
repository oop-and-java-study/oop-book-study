# Chapter 05

## SOLID

> 객체 지향의 기본이 되는 설계원칙, 변화에 유연하게 대처할 수 있는 설계원칙.

- SRP, ISP : 캡슐화를 통해 객체가 커지지 않도록하는 원칙이다. 한 기능의 변경이 다른 곳에 미치는 영향을 최소화한다.
- LSP, DIP: 추상화를 통해 OCP를 지원하는 원칙이다. OCP를 지키기 위해서는 LSP를 만족해야 하며, DIP를 통해 의존성을 관리함으로써 OCP를 구현할 수 있다.

```txt
   +-----------------------------------+
   |             OCP                   |
   | (확장에는 열리고, 수정에는 닫힌다)         |
   +-----------------------------------+
                ↑
                |
                |
   +-----------------------------------+
   |             LSP                   |
   | (서브타입은 부모 타입으로 대체 가능)       |
   +-----------------------------------+
                ↑
                |
                |
   +-----------------------------------+
   |             DIP                   |
   | (고수준 모듈은 저수준 모듈에 의존하지 않음) |
   +-----------------------------------+
```

### 단일 책임 원칙(SRP)

> 클래스는 단 한 개의 책임을 가져야한다.

- 객체 지향의 기본인 객체의 책임의 할당에 대한 원칙
- 책임의 개수가 많아질수록 한 책임의 기능 변화가 다른 책임에 주는 영향은 비례해서 증가하게 만들고, 이는 코드를 **절차 지향적으로 만들어 변경을 어렵게 만든다**.
- 단일 책임 원칙을 위반할 경우 재사용을 어렵게 만든다.
- **책임의 단위는 변화되는 부분과 관련된다.** 각각의 책임은 서로 다른 이유로 변경되고, 서로 다른 비율로 변경되는 특징이 있다.

#### SRP 위반

> **서로 다른 이유로 바뀌는 책임들이 한 클래스에 함께 포함되어 있다면 이 클래스는 단일 책임 원칙을 위반하고 있다고 볼 수 있다.**

    - 객체의 메서드를 누가 호출하는지를 확인해보면 단일 책임 원칙이 지켜지고 있는지 확인할 수 있다.
    - 클래스의 사용자들이 서로 다른 메서드들을 사용한다면 그들 메서드는 각각 다른 책임에 속할 가능성이 높고, 책임 분리 후보가 될 수 있다.

### 개방 폐쇄 원칙(OCP)

> 확장에는 열려 있어야하고, 변경에는 닫혀 있어야한다.

    -> 기능을 변경하거나 확장할 수 있으면서, 기능을 사용하는 코드는 수정하지 않는다.

- **개방 폐쇄 원칙은 변화가 예상되는 것을 추상화하여 변경의 유연함을 얻도록 해준다.**

#### OCP 구현 방법

- 확장되는 부분을 추상화

  ```java
  public class Data {
      private String content;

      public Data(String content) {
          this.content = content;
      }

      public String getContent() {
          return content;
      }
  }

  public interface ResponseSender {
      void send();
  }

  public class BasicResponseSender implements ResponseSender {
      private Data data;

      public BasicResponseSender(Data data) {
          this.data = data;
      }

      @Override
      public void send() {
          sendHeader();
          sendBody();
      }

      protected void sendHeader() {
          // 헤더 데이터 전송 예시
          System.out.println("헤더 전송: Content-Type: application/json");
      }

      protected void sendBody() {
          // 텍스트로 데이터 전송
          System.out.println("본문 전송: " + data.getContent());
      }
  }

  public class ZippedResponseSender extends BasicResponseSender {
      public ZippedResponseSender(Data data) {
          super(data);
      }

      @Override
      protected void sendBody() {
          // 압축처리한 데이터 전송
          System.out.println("압축된 본문 전송: " + compress(getData().getContent()));
      }

      private String compress(String content) {
          // 간단한 압축 예시 (실제 압축 알고리즘이 아님)
          return "압축된 데이터: " + content;
      }
  }
  // 사용 예시
  public class Main {
      public static void main(String[] args) {
          Data data = new Data("이것은 테스트 데이터입니다.");

          // 기본 응답 전송기 사용
          ResponseSender basicSender = new BasicResponseSender(data);
          basicSender.send();

          System.out.println("-----------------------");

          // 압축된 응답 전송기 사용
          ResponseSender zippedSender = new ZippedResponseSender(data);
          zippedSender.send();
      }
  }
  ```

- 상속을 통해 상위 클래스의 기능을 그대로 사용하면서 하위 클래스에서 일부 구현을 오버라이딩
  ``` java
  // 데이터 클래스 예시
  class Data {
  private String content;
  public Data(String content) {
  this.content = content;
  }
  public String getContent() {
  return content;
  }
  }
  // 기본 ResponseSender 클래스
  public class ResponseSender {
  private Data data;
  public ResponseSender(Data data) {
  this.data = data;
  }
  public Data getData() {
  return data;
  }
  public void send() {
  sendHeader();
  sendBody();
  }
  protected void sendHeader() {
  // 헤더 데이터 전송 예시
  System.out.println("헤더 전송: Content-Type: application/json");
  }
  protected void sendBody() {
  // 텍스트로 데이터 전송
  System.out.println("본문 전송: " + data.getContent());
  }
  }
  // 압축된 응답 전송기
  public class ZippedResponseSender extends ResponseSender {
  public ZippedResponseSender(Data data) {
  super(data);
  }
  @Override
  protected void sendBody() {
  // 압축처리한 데이터 전송
  System.out.println("압축된 본문 전송: " + compress(data.getContent()));
  }
  private String compress(String content) {
  // 간단한 압축 예시 (실제 압축 알고리즘이 아님)
  return "압축된 데이터: " + content;
  }
  }
  // 사용 예시
  public class Main {
  public static void main(String[] args) {
  Data data = new Data("이것은 테스트 데이터입니다.");
    
   ResponseSender responseSender = new ResponseSender(data);
  responseSender.send();
    
   System.out.println("-----------------------");
    
   ZippedResponseSender zippedResponseSender = new ZippedResponseSender(data);
  zippedResponseSender.send();
  }
  }

      ```

  -> 기존 클래스인 ResponseSender 클래스의 코드는 바뀌지 않음을 확인 할 수 있다.

#### OCP 위반 사례

- `instanceof` 와 같은 타입 확인 연산자가 사용된다. 타입 다운 캐스팅을 통해 특정 타입의 별도처리가 있다.
  - 별도 처리가 아닌 해당 메소드를 추상화하여 기존 `interface` 에 추가해야한다.
- 비슷한 `if-else` 블록 반복 사용

### 리스코프 치환 원칙(LSP)

> 상위 타입의 객체를 하위 타입의 객체로 치환해도 상위 타입을 사용하는 프로그램은 정상적으로 동작해야한다.

- OCP를 받쳐주는 다형성에 관한 원칙을 제공. 리스코프 치환 원칙을 위반하면 OCP 원칙 역시 위반된다.

#### LSP 위반 사례

- 하위 타입에서 상위 타입의 명세에서 벗어난 동작함.
  -> 하위 타입의 객체가 상위 타입의 객체를 올바르게 대체하지 않음.
  -> LSP 위반 시 명세에 기반해서 구현한 코드(상위 타입)는 비정상적으로 동작할 수 있다.
  - 명시된 명세에서 벗어난 값을 리턴한다.
  - 명시된 명세에서 벗어난 익셉션을 발생한다.
  - 명시된 명세에서 벗어난 기능을 수행한다.
- 올바른 확장을 하지 않아 OCP를 위반한다.

```java
	// 위반 예시
	public class Item {
		@Getter
		private final double price;

		public Item(double price) {
			this.price = price;
		}
	}

	public class Coupon {
		private final double discountRate = 1.0;

		public int calculateDiscountAmount(Item item) {
			// Item 종류에 따라 할인이 적용이 안 되어야하는 추가 요구 발생 시 해당 메소드에서
			// Item의 타입별로 처리하는 코드가 추가된다면 LSP가 위반 된다.
			if (item instanceof SpecialItem) {
				return 0;
			}
			return item.getPrice() * discountRate;
		}
	}
	// 해결 방안: 할인 적용 여부를 Coupon 클래스에서 하지 않고 Item 클래스에 추가한다.
	public class Item {
		@Getter
		private final double price;

		public boolean isDiscountAvailable() {
			return true;
		}

		public Item(double price) {
			this.price = price;
		}
	}
	public class NoDiscountlItem extends Item {
		@Override
		public boolean isDiscountAvailable() {
			return false;
		}
	}
	public class Coupon {
		private final double discountRate = 1.0;

		public int calculateDiscountAmount(Item item) {
			// 할인 가능 여부를 Coupon 클래스는 알바가 아니게 됨.
			if (!item.isDiscountAvailable) {
				return 0;
			}

			return item.getPrice() * discountRate;
		}
	}
```

### 인터페이스 분리 원칙(ISP)

> 인터페이스는 그 인터페이스를 사용하는 클라이언트를 기준으로 분리해야한다.

- 링크 과정이 있는 C, C++ 같은 언어 사용 시 ISP 위반 시 변경사항이 발생하면 재 컴파일이 필요하다.
- ISP 원칙은 인터페이스와 콘크리트 클래스의 재사용성을 높여준다.
- 클라이언트가 사용하는 기능을 중심으로 인터페이스를 분리하여 클라이언트로부터 발생하는 인터페이스 변경의 여파가 다른 클라이언트에 미치는 영향을 최소화한다.

### 의존 역전 원칙(DIP)

> 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안 된다. 저수준 모듈이 고수준 모듈에서 정의한 추상 타입에 의존해야한다.

- 저수준 모듈이 변경되더라도 고수준 모듈은 변경되지 않아야 프로그램의 변경을 유연하게 할 수 있다.
