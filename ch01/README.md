# 디자인 패턴과 리펙토링 

## 디자인 패턴이란?

- GoF( Gang of Four) 네 명의 학자가 기존의 많은 사례와 시스템등을 분석하여 좋은 설계라는 이런것이다 라는 23개 패턴을 제안

- 기존의 여러 시스템과 서비스를 기반으로 객체지향 프로그래밍에서 보다 유연하고, 확장성있는 설계가 가능한 예시를 제시

-  Each pattern describes a problem which occurs over and over again in our environment, and then describes the core of the solution to that problem, in such a way that you can use this solution a million times over, without ever doing it the same time twice.”
-- Christopher Alexander



## 디자인 패턴을 공부하면 좋은이유?




## 객체 지향 디자인 원칙 (Object Oriented Design Principle)

- 애플리케이션의 달라지는 부분을 찾아내고, 달라지지 않는 부분과 분리한다.
 새로운 요구사항이 있을때 마다 달라지는 부분은 분리해야 함

- 구현보다는 인터페이스에 맞춰서 프로그래밍 한다.
(Proram to an interface not an implementaion.)

- 상속보다는 합성을 사용한다. 
(Favor object composition over class inheritance.)

## 리펙토링이란?

- 외부에서 보는 프로그램의 기능은 변하지 않고 프로그램의 내부 구조를 개선하는것

- 프로그램의 동작이 변하지 않음을 증명하기 위해 유닛테스트가 중요함
테스트 -> 리펙토링 -> 테스트

- 디버깅은? 기능 추가는? 리펙토링이 아니다

## 
