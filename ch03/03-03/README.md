# Template Method

## 디자인 원리

- "변화하는 것과 변화하지 않는 것을 분리하고 다양하게 구현되는 것은 하위 클래스에 위임한다"

- 유사한 여러 클래스에 공통적인 부분을 추출하여 상위클래스에 구현하고 그렇지 않고 다양하게 구현될 수 있는 부분은 추상메서드로 선언하여 하위 클래스에 위임한다.

- 알고리즘의 골격을 구현한다. 프레임워크에서 많이 사용되는 패턴이다.

## 이전 코드 

- 세 가지 종류의 자동차는 동작 방식은 다르지만 동일한 과정의 순서로 이동한다.

AICar.java
```
public class AICar {

	public void startCar() {
		System.out.println("시동을 켭니다.");
	}
	
	public void turnOff() {
		System.out.println("시동을 끕니다.");
	}
	
	public void drive() {
		System.out.println("자율 주행합니다.");
		System.out.println("자동차가 스스로 방향을 바꿉니다.");
	}

	public void stop() {
		System.out.println("스스로 멈춥니다.");		
	}
}
```
ManualCar.java
```
public class ManualCar {

	public void startCar() {
		System.out.println("시동을 켭니다.");
	}
	
	public void turnOff() {
		System.out.println("시동을 끕니다.");
	}
	
	public void drive() {
		System.out.println("사람이 운전합니다.");
		System.out.println("사람이 핸들을 조작합니다.");		
	}
	
	public void stop() {
		System.out.println("브레이크를 밟아서 정지합니다.");		
	}
}
```
HybridCar.java
```
public class HybridCar {
	
	public void startCar() {
		System.out.println("시동을 켭니다.");
	}
	
	public void turnOff() {
		System.out.println("시동을 끕니다.");
	}
	
	public void drive() {
		System.out.println("사람이 조작하거나 자율 주행을 합니다.");
		System.out.println("사람이 핸들로 방향을 바꾸거나 자동으로 바뀝니다.");
	}

	public void stop() {
		System.out.println("스스로 멈추거나 사람이 브레이크를 밟습니다.");		
	}
}
```
CarTest.java
```
public class CarTest {

	public static void main(String[] args) {

		AICar myCar = new AICar();
		myCar.startCar();
		myCar.drive();
		myCar.stop();
		myCar.turnOff();
		System.out.println("*********************");
		
		ManualCar herCar = new ManualCar();
		herCar.startCar();
		herCar.drive();
		herCar.stop();
		herCar.turnOff();
		System.out.println("*********************");
		
		HybridCar yourCar = new HybridCar();
		yourCar.startCar();
		yourCar.drive();
		yourCar.stop();
		yourCar.turnOff();
		System.out.println("*********************");
	}
}
```

## 템플릿 메서드를 활용한 리펙토링 코드

1. 추상 클래스 만들기

    - 공통적으로 사용하는 메서드는 구현하고, 하위 클래스마다 다르게 구현되어야 하는것은 추상 메서드로 선언
Car.java
```
public abstract class Car {

	public void startCar() {
		System.out.println("시동을 켭니다.");
	}
	
	public void turnOff() {
		System.out.println("시동을 끕니다.");
	}
	
	public abstract void drive();
	public abstract void stop();
	public void washCar() {};
	
}
```
2. Car를 상속받아 각 차 클래스 구현하기

AICar.java
```
public class AICar extends Car{

	@Override
	public void drive() {
		System.out.println("자율 주행합니다.");
		System.out.println("자동차가 스스로 방향을 바꿉니다.");
	}

	@Override
	public void stop() {
		System.out.println("스스로 멈춥니다.");		
	}

	@Override
	public void washCar() {
		System.out.println("자동 세척합니다.");
	}
}
```
ManualCar.java
```
public class ManualCar extends Car{
	
	@Override
	public void drive() {
		System.out.println("사람이 운전합니다.");
		System.out.println("사람이 핸들을 조작합니다.");		
	}

	@Override
	public void stop() {
		System.out.println("브레이크를 밟아서 정지합니다.");		
	}
}
```
HybridCar.java
```
public class HybridCar extends Car{

	@Override
	public void drive() {
		System.out.println("사람이 조작하거나 자율 주행을 합니다.");
		System.out.println("사람이 핸들로 방향을 바꾸거나 자동으로 바뀝니다.");		
	}

	@Override
	public void stop() {
		System.out.println("스스로 멈추거나 사람이 브레이크를 밟습니다.");			
	}
}
```
3. 전체 시나리오를 구현한 템플릿 메서드 만들기
```
final public void run() {
		startCar();
		drive();
		stop();
		turnOff();
}
```

4. 테스트 프로그램
```
public class CarTest {

	public static void main(String[] args) {

		Car myCar = new ManualCar();
		myCar.run();
		System.out.println("*********************");
		
		Car herCar = new AICar();
		herCar.run();
		System.out.println("*********************");
		
		Car yourCar = new HybridCar();
		yourCar.run();
		System.out.println("*********************");
	}
}
```
5. Hook 메서드 추가하기

CarTest.java
```
public void washCar() {};
	
final public void run() {
	startCar();
	drive();
	stop();
	turnOff();
	washCar();  //hook method
}
```






