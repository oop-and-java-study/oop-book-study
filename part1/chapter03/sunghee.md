# Chapter 03

### 1. 상속
상속은 한 타입을 그대로 사용하면서 구현을 추가할 수 있도록 해주는 방법을 제공

이미 기본적인 기능을 제공하는 클래스를 확장해서 새로운 기능을 구현하여 좀 더 수월하게 새로운 기능 제공을 하고자 할 때 사용

예시: 기본 기능을 하는 Coupon 클래스와 이를 상속 받은 LimitPriceCoupon 클래스

- `상위 클래스(super class)` or `부모 클래스(parent class)`: 상속 대상이 되는 Coupon 클래스
- `하위 클래스(sub class)` or `자식 클래스(child class)`: 상속을 받은 LimitPriceCoupon 클래스

- 물려받을 수 있는 범위: private 범위를 갖는 메서드나 필드를 제외한 나머지 (`public` or `protected`)
- `overriding`: 하위 클래스에서 필요에 따라 상위 클레스에 정의된 메서드를 새롭게 구현하는 방식
---

### 2. 다형성
다형성은 한 객체가 여러 타입을 가질 수 있도록 함

하나의 객체가 타입A 혹은 타입B 혹은 타입C 에서 정의된 기능을 요청 받을 수 있음을 의미.

**[ 다형성 특징 ]**
- 상속받은 하위 클레스에서는 부모클래스 타입에 정의된 메서드의 실행을 요청할 수 있다.
- 상속받은 하위 타입의 객체는 부모 타입에 할당하는 것이 가능하다.

#### 2.1 인터페이스 상속과 구현 상속
자바에서는 다형성의 구현을 위해 타입을 상속받음

**[ 타입 상속 ]**
- 인터페이스 상속
  - 타입 정의만을 상속받는 것
  - 추상함수만을 가진 추상 클래스를 상속받는 경우
- 구현 상속
  - 클래스 상속을 통해 이루어짐
  - 일반적으로 상위 클래스에 정의된 기능을 재사용하기 위해 사용
  - 상속받은 하위 타입의 객체를 생성하여 부모타입에 할당할 경우, 해당 인스턴스의 메서드를 실행하면 하위 클래스에서 재정의한 메서드가 실행됨 (객체의 실제 타입이 하위클래스이기 때문!)

---
### 3. 추상타입과 유연함
추상화는 데이터나 프로세스 등을 의미가 비슷한 개념이나 표현으로 정의하는 과정

예시)
- FTP에서 파일을 다운로드
- 소켓으로 데이터 읽기
- DB 테이블 조회
```
class FtpLogFileDownloader { ... }

class SocketLogReader { ... }

class DbTableLogGateway { ... }
```
위 세 기능은 로그수집을 위한 공통의 **프로세스** 처리를 함.

아래와 같이 추상화를 할 수 있다.
```
interface LogCollector { 
    public void collect(); 
}
```

추상 타입은
- 오퍼레이션의 시그니처만을 정의
- 각 기능에 대한 상세 구현은 알 수 없음
- 따라서 구현을 제공하지 않는 자바의 인터페이스 혹은 추상클래스를 이용하여 정의

#### 3.1 추상 타입과 실제 구현의 연결
추상타입과 실제 구현 클래스는 상속을 통해 연결

`콘크리트 클래스(concrete class)`: 인터페이스에 정의된 기능의 실제 구현을 제공하는 클래스. 구상클래스.

#### 3.2 추상 타입을 이용한 구현 교체의 유연함

```
public class FlowController {
    public void process() {
        FileDataReader reader = new FileDataReader();
        byte[] data = reader.read();
        
        Encryptor encrytor = new Encryptor();
        byte[] encryptedData = encrytor.encrypt(data);
        
        FileDataWriter writer = new FileDataWriter();
        writer.write(encryptedData);
    }
}
```
요구조건의 변경
> 파일 뿐 아니라 소켓을 통해 읽어온 데이터를 암호화 할 수 있어야 한다

나쁜 소스
```
public class FlowController {
    private boolean useFile;
    
    public FlowController(boolean useFile) {
        this.useFile = useFile;
    }
    
    public void process() {
        byte[] data = null;
        
        if (useFile) {
            FileDataReader fileReader = new FileDataReader();
            data = fileReader.read();
        } else {
            SocketDataReader socketReader = new SocketDataReader();
            data = socketReader.read();
        }
        
        Encryptor encrytor = new Encryptor();
        byte[] encryptedData = encrytor.encrypt(data);
        
        FileDataWriter writer = new FileDataWriter();
        writer.write(encryptedData);
    }
}
```

> 파일에서 바이트 데이터를 읽어오는 기존 요구사항과, 소켓에서 바이트 데이터를 읽어오는 추가 요구사항의 공통점을 찾기
> > 어떤 곳으로부터 바이트 데이터 읽기

위 공통점으로 추상화한 타입의 소스를 생성
```
public interface ByteSource {
    public byte[] read();
}
```
추상 타입을 상속 받아 콘크리트 클래스를 구현
```
public class FileDataReader implements ByteSource {
    public byte[] read() { ... }
}

public class SocketDataReader implements ByteSource {
    public byte[] read() { ... }
}
```
좋은 소스 ver1.
```
ByteSource source = null;
if (useFile) {
    source = new FileDataReader();
} else {
    source = new SocketDataReader();
}
byte[] data = source.read();
```
좋은 소스 ver2.
1. ByteSource 타입의 객체를 생성해주는 책임을 갖는 ByteSourceFactory 클래스를 구현하고
```
public class ByteSourceFactory {
    public ByteSource create() { // 객체 생성 기능을 위한 오퍼레이션 정의
       if (useFile) {
            source = new FileDataReader();
        } else {
            source = new SocketDataReader();
        }
    }
    
    public boolean useFile() {
        String useFileVal = System.getProperty("useFile");
        return useFileVal != null && Boolean.valueOf(useFileVal);
    }
    
    // 싱글톤 패턴 적용
    private static ByteSourceFactory instance = new ByteSourceFactory();
    public static ByteSourceFactory getInstance() {
        return instance;
    }
    
    private ByteSourceFactory() {}
}
```
2. FlowController 클래스가 ByteSourceFactory 를 사용하도록 코드를 수정
```
public class FlowController {
    public void process() {
        // 데이터 읽기 객체 생성
        ByteSource source = ByteSourceFactory.getInstance().create();
        // 흐름제어: 1 읽기
        byte[] data = source.read();
        
        Encryptor encrytor = new Encryptor();
        // 흐름제어: 2 암호화
        byte[] encryptedData = encrytor.encrypt(data);
        
        FileDataWriter writer = new FileDataWriter();
        // 흐름제어: 3 쓰기
        writer.write(encryptedData);
    }
}
```
새로운 요구사항의 추가 시
- 새로운 ByteSource 구현클래스는 추가됨
- FlowController 클래스의 코드는 영향 X
- FlowController 의 제어 흐름이 변경될 때, ByteSource 객체 생성 부분은 영향 없이 FlowController만 변경하면 됨

> 예시 소스에서는 상세 구현을 하는 ByteSource 보다 전체 비스니스 로직의 흐름을 담당하는 FlowController 의 재사용성이 더 중요하다. <br> 
> 따라서 상위 수준의 로직을 재사용할 수 있도록 설계하는 것이 중요하다.

#### 3.3 변화되는 부분을 추상화하기
변화되는 부분이 추상화 되기 좋은 후보!<br>
이는 주로 동일 구조를 같은 **if-else 블록**으로 드러난다.<br>
=> **추상화**를 통해, 추상 타입을 사용하는 코드에는 영향을 주지 않고, 추상 타입의 실제 구현을 변경하여 새로운 요구 반영이 가능

#### 3.4 인터페이스에 대고 프로그래밍하기 (`program to interface`)
이 역시 추상화를 통한 유연함을 얻기 위한 규칙 <br>
적절한 인터페이스의 사용이 중요한데, 이 인터페이스는 최초 설계 뿐 아니라 요구사항의 변경에 의해 새롭게 도출되기도 함 <br>
**변화 가능성이 높은 콘크리트 클래스 대신 이를 추상화한 인터페이스를 사용하여, 변경의 유연함을 취하는 게 중요**

#### 3.5 인터페이스는 인터페이스 사용자 입장에서 만들기
인터페이스의 명칭을 명명할 때는 **사용자의 입장**에서 **추상화된 핵심 기능**을 중심으로 하여 명명한다

#### 3.6 인터페이스와 테스트
인터페이스를 활용한 개발은 테스트 단위 간의 의존성을 줄여 Mock객체 활용을 통한 별도 테스트가 가능하다.