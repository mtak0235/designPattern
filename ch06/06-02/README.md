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

- **Colleague** : Mediator의 참조자를 가지고 있고, 자신의 상태가 변하거나 다른 객체와의 협력이 필요할때 Mediator에게 알림

- **ConcreteMediator** : Colleague간의 이루어지는 협력을 구현하고, 자신의 Colleague들을 관리한다.

## 프로그램 예제

Mediator.java
```
public interface Mediator {
    public abstract void createColleagues();
    public abstract void colleagueChanged(Colleague colleague);
}
```

Colleague.java
```
public interface Colleague {
    public abstract void setMediator(Mediator mediator);
    public abstract void setColleagueEnabled(boolean enabled);
}
```

ColleagueButton.java
```
public class ColleagueButton extends Button implements Colleague {
    private Mediator mediator;
    public ColleagueButton(String caption) {
        super(caption);
    }
    public void setMediator(Mediator mediator) {            // Mediator를 보관
        this.mediator = mediator;
    }
    public void setColleagueEnabled(boolean enabled) {      // Mediator가 유효/무효를 지시한다.
        setEnabled(enabled);
    }
}
```

ColleagueCheckbox.java
```
public class ColleagueCheckbox extends Checkbox implements ItemListener, Colleague {
    private Mediator mediator;
    public ColleagueCheckbox(String caption, CheckboxGroup group, boolean state) {  // 생성자
        super(caption, group, state);
    }
    public void setMediator(Mediator mediator) {            // Mediator를 보관
        this.mediator = mediator;
    }
    public void setColleagueEnabled(boolean enabled) {      // Mediator가 유효/무효를 지시한다.
        setEnabled(enabled);
    }
    public void itemStateChanged(ItemEvent e) {             // 상태가 변하면 Mediator에게 통지
        mediator.colleagueChanged(this);
    }
}
```

ColleagueTextField.java
```
public class ColleagueTextField extends TextField implements TextListener, Colleague {
    private Mediator mediator;
    public ColleagueTextField(String text, int columns) {   // 생성자
        super(text, columns);
    }
    public void setMediator(Mediator mediator) {            // Mediator를 보관
        this.mediator = mediator;
    }
    public void setColleagueEnabled(boolean enabled) {      // Mediator가 유효/무효를 지시한다.
        setEnabled(enabled);
        setBackground(enabled ? Color.white : Color.lightGray);
    }
    public void textValueChanged(TextEvent e) {             // 문자열이 변하면 Mediator에게 통지
        mediator.colleagueChanged(this);
    }
}
```

LoginFrame.java
```
public class LoginFrame extends Frame implements ActionListener, Mediator {
    private ColleagueCheckbox checkGuest;
    private ColleagueCheckbox checkLogin;
    private ColleagueTextField textUser;
    private ColleagueTextField textPass;
    private ColleagueButton buttonOk;
    private ColleagueButton buttonCancel;

    // 생성자
    // Colleague들을 생성해서 배치한 후에 표시를 실행한다.
    public LoginFrame(String title) {
        super(title);
        setBackground(Color.lightGray);
        // 레이아웃 매니저를 사용해서 4*2의 그리드를 만든다.
        setLayout(new GridLayout(4, 2));
        // Colleague들의 생성
        createColleagues();
        // 배치
        add(checkGuest);
        add(checkLogin);
        add(new Label("Username:"));
        add(textUser);
        add(new Label("Password:"));
        add(textPass);
        add(buttonOk);
        add(buttonCancel);
        // 유효/무효의 초기지정
        colleagueChanged(checkGuest);
        // 표시
        pack();
        show();
    }
	
    // Colleague들을 생성한다.
    public void createColleagues() {
        // 생성
        CheckboxGroup g = new CheckboxGroup();
        checkGuest = new ColleagueCheckbox("Guest", g, true);
        checkLogin = new ColleagueCheckbox("Login", g, false);
        textUser = new ColleagueTextField("", 10);
        textPass = new ColleagueTextField("", 10);
        textPass.setEchoChar('*');
        buttonOk = new ColleagueButton("OK");
        buttonCancel = new ColleagueButton("Cancel");
        // Mediator의 세트
        checkGuest.setMediator(this);
        checkLogin.setMediator(this);
        textUser.setMediator(this);
        textPass.setMediator(this);
        buttonOk.setMediator(this);
        buttonCancel.setMediator(this);
        // Listener의 세트
        checkGuest.addItemListener(checkGuest);
        checkLogin.addItemListener(checkLogin);
        textUser.addTextListener(textUser);
        textPass.addTextListener(textPass);
        buttonOk.addActionListener(this);
        buttonCancel.addActionListener(this);
    }

    // Colleague로부터의 통지로 각 Colleague의 유효/무효를 판정한다.
    public void colleagueChanged(Colleague c) {
        if (c == checkGuest || c == checkLogin) {
            if (checkGuest.getState()) {             // Guest 모드
                textUser.setColleagueEnabled(false);
                textPass.setColleagueEnabled(false);
                buttonOk.setColleagueEnabled(true);
            } else {                                 // Login 모드
                textUser.setColleagueEnabled(true);
                userpassChanged();
            }
        } else if (c == textUser || c == textPass) {
            userpassChanged();
        } else {
            System.out.println("colleagueChanged:unknown colleague = " + c);
        }
    }
    // textUser 또는 textPass의 변경이 있었다.
    // 각 Colleague의 유효/무효를 판정한다.
    private void userpassChanged() {
        if (textUser.getText().length() > 0) {
            textPass.setColleagueEnabled(true);
            if (textPass.getText().length() > 0) {
                buttonOk.setColleagueEnabled(true);
            } else {
                buttonOk.setColleagueEnabled(false);
            }
        } else {
            textPass.setColleagueEnabled(false);
            buttonOk.setColleagueEnabled(false);
        }
    }
    public void actionPerformed(ActionEvent e) {
        System.out.println("" + e);
        System.exit(0);
    }
}
```

Main.java
```
public class Main {
    static public void main(String args[]) {
        new LoginFrame("Mediator Sample");
    }
}
```
