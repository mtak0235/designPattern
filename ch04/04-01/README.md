# Decorator

## 디자인 원리

- 상속을 하지 않고도 확장성 있게 클래스를 설계

- 상속은 클래스간의 종속성이 높아짐 : 상위 클래스의 변경이 하위 클래스에 영향을 줌

- 기존의 코드를 변경하지 확장으로 새로운 기능을 추가해보자

    **OCP (Open-Closed Priciple)**
    
    **클래스는 확장에는 열려 있어야 하지만 변경에는 닫혀 있어야 한다.**

## Class Diagram

![decorator](./img/decorator.JPG)

## 객체간의 협력

- **Component** : 동적으로 추가할 서비스를 가질 수 있는 객체 정의

- **ConcreteComponent** : 추가적인 서비스가 필요한 실제 객체

- **Decorator** : Component의 참조자를 관리하면서 Component에 정의된 인터페이스를 만족하도록 정의

- **ConcreteDecorator** : 새롭게 추가되는 서비스를 실제 구현한 클래스로 addBehavior()를 구현한다.

Decorator 패턴에서는 Component와 Decorator를 동일하게 사용할 수 있지만, 실제 기능을 제공하는 것은 Component 임

## 예제

다양한 커피를 만들어 보자. 커피에 부가적인 기능이 추가된 커피를 만들기위해 상속을 사용하지 않고 장식자들을 추가한다.


Coffee.java
```
public abstract class Coffee {
	
	public abstract void brewing();
}
```

EtiopiaAmericano.java
```
public class EtiopiaAmericano extends Coffee{

	@Override
	public void brewing() {
		System.out.print("EtiopiaAmericano ");
	}

}
```

KenyaAmericano.java
```
public class KenyaAmericano extends Coffee{

	@Override
	public void brewing() {
		System.out.print("KenyaAmericano ");
	}

}
```

Decorator.java
```
public abstract class Decorator extends Coffee{

	Coffee coffee;
	public Decorator(Coffee coffee){
		this.coffee = coffee;
	}
	
	@Override
	public void brewing() {
		coffee.brewing();
	}

}
```

Latte.java
```
public class Latte extends Decorator{

	public Latte(Coffee coffee) {
		super(coffee);
	}
	
	public void brewing() {
		super.brewing();
		System.out.print("Adding Milk ");
	}
}
```

Mocha.java
```
public class Mocha extends Decorator{

	public Mocha(Coffee coffee) {
		super(coffee);
		// TODO Auto-generated constructor stub
	}

	public void brewing() {
		super.brewing();
		System.out.print("Adding Mocha Syrup ");
	}
}
```

WhippedCream.java
```
public class WhippedCream extends Decorator{

	public WhippedCream(Coffee coffee) {
		super(coffee);
	}

	public void brewing() {
		super.brewing();
		System.out.print("Adding WhippedCream ");
	}
}
```

CoffeeTest.java
```
public class CoffeeTest {

	public static void main(String[] args) {

		Coffee kenyaAmericano = new KenyaAmericano();
		kenyaAmericano.brewing();
		System.out.println();
		
		Coffee kenyaLatte = new Latte(kenyaAmericano);
		kenyaLatte.brewing();
		System.out.println();
		
		Mocha kenyaMocha = new Mocha(new Latte(new KenyaAmericano()));
		kenyaMocha.brewing();
		System.out.println();
		
		WhippedCream etiopiaWhippedMocha = 
				new WhippedCream(new Mocha(new Latte( new EtiopiaAmericano())));
		etiopiaWhippedMocha.brewing();
		System.out.println();
		
	}

}
```

## Java I/O 

- 많은 클래스들이 제공되고 있음

- 실제 I/O 가 일어나는 클래스와 이를 감싸서 보조적인 기능을 제공하는 클래스 (보조 클래스, 이차 클래스, Wrapper 클래스)가 있음

- 예 )   BufferedReader br = new BufferedReader(new InputStreamReader(socket.getInputStream()));

         BufferedReader br2 = new BufferedReader(new InputStreamReader(System.in));
