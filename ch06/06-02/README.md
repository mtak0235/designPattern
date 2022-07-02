# Mediator

## 디자인 원리

![mediator1](./img/mediator.PNG)

- 여러 객체(colleague) 상호 간의 작용이 많고, 이 작용들이 다른 colleague에도 영향을 주는 경우 사용

- UI 프로그래밍에서 많이 사용되는 방식으로 widget 상호간에 주고 받을 메세지를 각 widget이 주고 받는것이 아닌 한 객체(medaitor)가 전담하여 처리하도록 하는 방법 

- 객체 지향 방법론에서는 객체의 관련된 처리를 객체 내부에서 하는것이 맞지만, 그런 경우에 상호작용이 급증하게 된다면 코드의 수정이 어렵다.

- Mediator 객체가 상호작용을 제어하고 조율하게 함. 각 객체는 다른 객체의 참조자는 알 필요없고 Mediator만 알면 되므로 colleague간의 종속성이 약화되고 결합도가 줄어듬

- N:N의 관계를 1:N의 관계로 바꿀 수 있음 (counselor)


## 클래스 다이어그램 

![mediator2](./img/mediator2.PNG)


## 프로그램 예제
