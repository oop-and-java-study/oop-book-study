# 옵저버 패턴

> 한 객체의 상태 변화를 정해지지 않은 여러 다른 객체에 통지하고 싶을 때 사용되는 패턴

- 주제 클래스 변경 없이 상태 변경을 통지 받을 옵저버를 추가할 수 있다는 장점이 있다.
- 주제 객체가 옵저버 호출 시 상태 값을 전달한다. 옵저버 클래스는 필요 시 전달 받은 상태 값을 사용하거나 콘크리트 주제 클래스에 대한 의존을 갖는다.

## 구성 요소

- **주제(subject)**
  - 옵저버 목록 관리: 옵저버 등록 및 제거 메소드 제공
  - 상태 변경 고지: 등록된 옵저버에 상태 변경 내역을 알림
- **옵저버(observer)**

## 고려사항

1. 옵저버에 통지하는 시점을 결정하는 주체
2. 옵저버의 인터페이스 개수
   - 한 주제 객체가 통지할 수 있는 상태 변경 내역의 종류가 다양한 경우에는 별도 옵저버 인터페이스로 분리해서 구현하는 것을 권장
3. 통지 시점에서 주제 객체의 상태에 결함 여부
   - 옵저버 객체가 올바르지 않은 상태값을 참조하는 문제가 일어나지 않도록 주의해야함.
   - 상태 변경과 통지기능에 템플릿 메서드 패턴을 적용하여 실행 순서를 보장하는 방법도 있음.
4. 옵저버 객체의 실행에 대한 제약 규칙
   - 옵저버 인터페이스 정의 시 매서드 실행에 대한 명확한 기준이 필요하다.

## 구현

```java
	public abstract class StatusSubject {
		private List<StatusObserver> observers = new ArrayList<StatusObserver>();

		public void add(StatusObservere observer) {
			observers.add(observer);
		}

		public void remove(StatusObserver observer) {
			observers.remove(observer);
		}

		public void notifyStatus(Status status) {
			for (StatusObserver observer : observers) {
				observer.onAbnormalStatus(status); // 상태를 옵저버 객체에 전달
			}
		}
	}

	public class StatusChecker extends StatusSubject {
		public void check() {
			Status status = loadStatus();

			if (status.isNotNormal()) {
				super.notifyStatus(status);
			}
		}

		private Status loadStatus() {
		}
	}

	public interface StatusObserver {
		void onAbnormalStatus(Status status);
	}

	public class StausEmailSender implements StatusObserver {
		private StutusChecker statusCheck;
		@Override
		public void onAbnormalStatus(Status status) {
			// 전달받은 상태 값을 사용해야 하는 경우
			if (status.isFault()) {
				sendEmail(status);
			}
			// 전달 받은 상태 객체로는 옵저버의 기능을 구현할 수 없을 경우
			// 필요한 데이터를 직접 가져오도록 함
			if (status.isFault() && statusCheck.isContinousFault()) {
				sendEmail(status);
			}
			sendEmail(status);
		}

		private void sendEmail(Status status) {
			// 이메일 전송 코드
		}
 	}
```

![어댑터예제](/images/observer.png)
