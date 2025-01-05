# 프록시 패턴

> 실제 객체를 대신하는 프록시 객체를 사용해서 실제 객체의 생성이나 접근등을 제어할 수 있도록 하는 패턴(구조패턴)

## 의도

- 실제 객체 대한 접근을 제어를 목적으로 한다.
- 클래스의 메인 로직 이전이나 이후에 무언가를 실행해야 하는 경우 **프록시는 해당 클래스를 변경하지 않고도 이 무언가를 수행할 수 있도록 한다**. 프록시는 원래 클래스와 같은 인터페이스를 구현하므로 실제 서비스 객체를 기대하는 모든 클라이언트에 전달될 수 있다.

### 구현

- 조립: 가상 프록시를 구현하고자 할때 적합하다. 실제 객체의 생성이 프록시 객체에서 이루어진다.

  ```java
  public interface Image {
  	public void draw();
  }
  public class ProxyImage implements Image {
  	private String path;
  	private RealImage image;

  	public ProxyImage(String path) [
  		this.path = path;
  	]
  	public void draw() {
  		if (image == null) {
  			this.image = new RealImage(path) // 최초 접근 시 객체 생성
  	//  실제 요청을 보내야 하는 경우에만 실제 서비스 객체에 위임합니다.
  	// 화면에 표시되지 않는 이미지를 로딩하기 위해 불필요하게 메모리를 낭비하는 상황을 방지한다.
  		}
  		image.draw() // RealImage 객체에 위임
  	}
  }
  ```

- 상속: 접근 제어를 위한 목적인 보호 프록시의 경우 보호 프록시 객체를 생성할 때 실제 객체를 전달하면 된다.
  ```java
  public class ProtectedService extends Service {
  	@Override
  	public void SomeMethod() {
  		if (!CurrentContext.getAuth().isAdmin()) {
  			throw new AccessDeniedException();
  		}
  		super.someMethod(); //권한 확인 후 실제 객체의 메소드 실행.
  	}
  }
  ```
