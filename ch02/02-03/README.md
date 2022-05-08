# Singleton

## 디자인 원리

- 클래스의 인스턴스가 오직 하나만이 존재해야 하고, 이에 대한 접근도 동일한 인터페이스를 통해 가능

- 인스턴스가 여러 개가 되면 오류가 생길 수 있고, 불필요한 자원들이 생성되고, 일관성이 없어지는 일이 발생하는 경우
 (Calendar, Logger, Connection pool, 레지스트리 설정,  학교 등등...)

- 전역 변수를 쓰는 것은 안 좋은 프로그래밍 방법 

- 자바에서는 static을 활용함

## Class Diagram

![singleton](./img/singleton.JPG)


## 간단한 Singleton 예제 : 게으른 인스턴스 생성 (lazyinstantiation)

Singleton.java
```
public class Singleton {

	private static Singleton instance;
	
	private Singleton() {
		
	}
	
	public static Singleton getInstance() {
		if(instance == null) {                   // 멀티 thread 에서는 문제가 될 수도...
			instance = new Singleton();
		}
		return instance;
	}
}
```

SingletonTest.java
```
public class SingletonTest {

	public static void main(String[] args) {

		Singleton instanceA = Singleton.getInstance();
		Singleton instanceB = Singleton.getInstance();
		
		System.out.println(instanceA == instanceB);
	}

}
```

## multi-thread 에서는 Singleton 패턴에서도 두 개의 인스턴스가 생성될 수 있음

- 두 thread 모두 instance == null 에서 switch가 발생하면 두 개의 instance가 생성됨

- thread-safe를 보장하는 코드

### 해결 방법

1. 동기화 방식

```
public static sychronized Singleton getInstance() {
	if(instance == null) {                   
		instance = new Singleton();
	}
	return instance;
}

```
 => 매번 동기화에 대한 overhead 가 발생할 수 있음

2. 인스턴스를 처음부터 생성 
```
public class Singleton {

	private static Singleton instance = new Singleton();
	
	private Singleton() {
		
	}
	
	public static Singleton getInstance() {
		return instance;
	}
}
```
 => 인스턴스를 사용하는 시점에 생성하는 것보다는 overhead 가능성

3. DCL (Double-Checked Locking) 방법

```
public class Singleton {

	private volatile static Singleton instance;
	
	private Singleton() {}		
	
	public static Singleton getInstance() {
		
		if(instance == null) {
			synchronized(Singleton.class){
				instance = new Singleton();
			}
		}
		return instance;
	}
}
```
=> 이미 생성된 경우는 메서드 Locking이 발생하지 않음 


