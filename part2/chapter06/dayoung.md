### **Chapter 6. DI와 서비스 로케이터**

#### **어플리케이션 영영과 메인 영역**
시스템 설계 시, 코드를 크게 나누었을 때, 두 가지 영역으로 나눌 수 있다.
+ 메인 영역
  + 어플리케이션 영역에서 사용될 객체를 생성
  + 각 객체간 의존 관계 설정
  + 어플리케이션 실행
+ 어플리케이션 영역
  + 요구사항에 따른 기능 실행

이때, 메인 영역에서 의존 관계를 생성하는 방법은 두 가지가 존재한다.
+ 서비스 로케이터: 로케이터를 통해 필요로 하는 객체를 직접 찾는 방식
+ 의존 주입(DI): 외부에서 의존하는 객체를 직접 넣어 의존 관계를 만드는 방식

<br>

---

#### **서비스 로케이터**
안드로이드 플랫폼과 같이 프레임워크의 제약으로 DI 패턴을 적용할 수 없는 경우가 있는데, 이 때 서비스 로케이터를 이용해 의존 객체를 생성하게 된다.

서비스 로케이터를 구현하는 방식은 다음과 같다.
+ 객체등록 방식제공
    ```java
    public class ServiceLocator{
        private JobQueue jobQueue;
        private Transcoder transcoder;

        public ServiceLocator(JobQueue jobQueue, Transcoder transcoder){
            this.jobQueue = jobQueue;
            this.transcoder = transcoder;
        }

        private static ServiceLocator isntance;
        public static void load(ServiceLocator locator){
            ServiceLocator.isntance = locator;
        }

        public static ServiceLocator getInstance(){
            return isntance;
        }
    }
    ```
+ 상속 방식
    ```java
    public abstract class ServiceLocator{
        public abstract JobQueue jobQueue;
        public abstract Transcoder transcoder;

        private static ServiceLocator isntance;
        public static ServiceLocator getInstance(){
            return isntance;
        }
    }
    // 하위클래스
    public class MyServiceLocator extends ServiceLocator{
        private JobQueue jobQueue;
        private Transcoder transcoder;

        public MyServiceLocator{
            super();
            this.jobQueue = new FileJobQueue();
            this.transcoder = new FfmpegTranscoder();
        }

        @Override
        public jobQueue getJobQueque(){ return jobQueue; }
        @Override
        public Transcoder getTranscoder(){ return transcoder; }

    }
    ```
+ 제네릭/템플릿 방식
    ```java
    public class ServiceLocator{
        private static Map<Class<?>, Object> objectMap = new HashMap<>();
        
        public static <T> T get(Class<T> klass){
            return (T) objectMap.get(klass)
        }
        public static void regist(Class<?> klass, Object object) {
            objectMap.put(klass, obeject);
        }
    }
    ```

**서비스 로케이터 단점**
+ 동일 타입의 객체가 다수 필요 시, 객체별로 제공 메서드 생성 필요
+ 인터페이스 분리 원칙 위반

<br>

---

#### **의존성 주입 (Depnedency Injection)**
서비스 로케이터의 단점을 보완하여 나온 것이, 외부에서 의존 받을 객체를 주입 받는 DI 방식이다. 더 나아가 XML 등 외부 파일로 객체 생성/조립에 대한 설정을 받아와, 어플리케이션 실행 시 초기화할 수 있도록 한다. 대표적으로 Spring 프레임워크가 이와 같이 작동하는 대표적인 DI 프레임워크이다.

DI 적용 방식
+ 생성자 방식
  + 생성자를 통해 의존 객체 전달
  + 객체 생성 시점에 필요한 모든 객체를 준비
  + 객체 생성 시점에 이상 여부 판단 가능
+ 설정 메서드 방식
  + set 메서드로 의존 객체 전달
  + 객체 생성 시점에 의존 객체가 생성되어 있지 않아도 됨
  + 메서드 이름을 통해 어떤 의존 객체가 설정되는지 추측하기 쉬움
  + `nullPointerException` 발생할 수 있음