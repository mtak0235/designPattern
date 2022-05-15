# Factory Method

## 디자인 원리
  " 구체적인 것에 의존하지 말고 추상적인 것에 의존한다" 
  
  - 객체 생성에 new 를 사용하여 구체 클래스 인스턴스를 생성하게 되면 
  대상의 인스턴스가 변경되었을 때 프로그램이 수정되어야 함

  예) Car car = new Car("Sonata");  // 차종에 따라 여러 차가 생성될 수 있음

  - 여러 인스턴스가 다양하게 생성될 수 있는 상황에서는 팩토리 메서드를 사용한다.

  - 생성될 수 있는 여러 객체를 추상화 하고, 팩토리에서는 추상 클래스를 활용하고 생성하는 메서드를 제공한다. 클라이언트는 실제 인스턴스와 상관없이, 팩토리가 제공해주는 생성 메서드 (에를 들얼 createCar())를 사용하면 된다. 구체적인 클래스에 종속되지 않음

## 이전의 코드 ( 구체적인 클래스 기반의 코드 )

Car.java
```
package factory.before;

public  class Car {

	public static final String SONATA = "Sonata";
	public static final String GRANDEUR = "Grandeur";
	public static final String GENESIS = "Genesis";
	
	String productName;
	
	public Car(String productName) {
		this.productName = productName;
	}
	
	public String toString() {
		return productName;
	}
}
```

CarTest.java (클라이언트 코드)
```
public class CarTest {
	
	public static void main(String[] args) {
		
		CarTest test = new CarTest();
		Car car = test.produceCar("Sonata");
			
		System.out.println(car);
	}
	
	public Car produceCar(String name) {
	
		Car car = null;
		
		if( name.equalsIgnoreCase(Car.SONATA)) {
			car = new Car(Car.SONATA);
		}
		else if( name.equalsIgnoreCase(Car.GRANDEUR)) {
			car = new Car(Car.GRANDEUR);
		}
		else if( name.equalsIgnoreCase(Car.GENESIS)) {
			car = new Car(Car.GENESIS);
		}
		else {
			car = new Car("noname");
		}
		
		return car;
	}
}
```
=> 분류코드를 사용함. 생성하는 인스턴스가 증가면 코드가 점점 길어짐

## 간단한 팩토리로 리펙토링 하기  

1. Car를 상위 클래스로 만들고 각각의 제품을 하위 클래스로 만든다.

2. 팩토리에서 인스턴스를 생성한다.

Car.java
```
public abstract class Car {
		
	String productName;
	
	public Car(String productName) {
		this.productName = productName;
	}
		
	public String toString() {
		return productName;
	}
}
```

Sonata.java
```
public class Sonata extends Car{

	public Sonata(String productName) {
		super(productName);
	}

}
```
Grandeur.java
```
public class Grandeur extends Car{
	
	public Grandeur(String productName) {
		super(productName);
	}
}
```
Genesis.java
```
public class Genesis extends Car{
		
	public Genesis(String productName) {
		super(productName);
	}
}
```
CarFactory.java
```
public class CarFactory {

	public Car createCar(String name) {
		
		Car car = null;
		
		if( name.equalsIgnoreCase("sonata")) {
			car = new Sonata(name);
		}
		else if( name.equalsIgnoreCase("grandeur")) {
			car = new Sonata(name);
		}
		else if( name.equalsIgnoreCase("genesis")) {
			car = new Sonata(name);
		}
		
		return car;
	}
}
```

## 팩토리를 추상화 하기

1. 팩토리를 추상화 하여 여러 팩토리가 상속받고 다양한 제품군을 만들 수 있다.

2. 팩토리에서 제품을 생성하는 과정이 다양한 공정과정을 거치게 되면 템플릿 메서드 방식을 사용할 수 있다.

CarFactory.java
```
public abstract class CarFactory {

	final public Car manufaturingCar(String name) {
		
		Car car;
		prepareOthers();
		car = createCar(name);
		washCar();
		
		return car;
	}
		
	public void prepareOthers() {
		System.out.println("prepare others");
	}
	
	public void washCar() {
		System.out.println("wash car");
	}
	
	public abstract Car createCar(String name);
}
```
HyundaiFactory.java
```
public class HyundaiFactory extends CarFactory{
	
	@Override
	public Car createCar(String name) {
		Car car = null;

		if( name.equalsIgnoreCase("sonata")) {
			car = new Sonata(name);
		}
		else if( name.equalsIgnoreCase("grandeur")) {
			car = new Sonata(name);
		}
		else if( name.equalsIgnoreCase("genesis")) {
			car = new Sonata(name);
		}
		System.out.println(car + " created");
		
		return car;
	}

}
```


![fatorymethod](./img/fatorymethod.JPG)

- 상위 클래스에서 추상 팩토리 메서드를 제공하고 하위 클래스에서 이를 구현하여 구체 클래스를 생성한다. 따라서 클라이언트가 





사용하게 되는 상위 메서드는 추상화되어 있고, 실제 객체가 생성되는 하위 클래스와 분리되어 유연성이 제공된다.
