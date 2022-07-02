# Proxy

## 디자인 원리 

- 실제 객체에 대한 대리자를 둔다.

- 대리자의 종류

    원격 프록시 (Remote Proxy) : 서로 다른 주소 공간에 존재하는 원격 객체를 대리하는 로컬 객체, Java RMI 기능

    가상 프록시 (virtual proxy) : 실제 객체가 모두 생성되기 전에 대신해주는 프록시

    보호 프록시 (Protection Proxy) : 실제 객체에 대한 접근 권한을 제어하기 위한 경우 사용

![proxy](./img/proxy.PNG)

- 실제 객체와 프록시는 동일한 메서드를 제공함

- 클라이언트가 호출 하는 메서드는 프록시의 메서드 

- 프록시에서 처리하는 경우도 있고, 실제 객체 전달되어 처리 되는 경우도 있음

## 프로그램 예제

Printable.java
```
public interface Printable {
    public abstract void setPrinterName(String name);   // 이름의 설정
    public abstract String getPrinterName();            // 이름의 취득
    public abstract void print(String string);          // 문자열 표시(프린트아웃)
}
```

PrinterProxy.java
```
public class PrinterProxy implements Printable {
    private String name;            // 이름
    private Printer real;           // "본인"
    public PrinterProxy() {
    }
    public PrinterProxy(String name) {      // 생성자
        this.name = name;
    }
    public synchronized void setPrinterName(String name) {  // 이름의 설정
        if (real != null) {
            real.setPrinterName(name);  // "본인"에게도 설정한다.
        }
        System.out.println("proxy : setPrinterName()");
        this.name = name;
    }
    public String getPrinterName() {    // 이름의 취득
    	System.out.println("proxy : getPrinterName()");
    	return name;
    }
    public void print(String string) {  // 표시
        realize();
        real.print(string);
    }
    private synchronized void realize() {   // "본인"을 생성
        if (real == null) {
            real = new Printer(name);
        }
    }
}
```

Printer.java
```
public class Printer implements Printable {
    private String name;
    public Printer() {
        heavyJob("Printer의 인스턴스를 생성중");
    }
    public Printer(String name) {                   // 생성자
        this.name = name;
        heavyJob("Printer의 인스턴스(" + name + ")를 생성중");
    }
    public void setPrinterName(String name) {       // 이름의 설정
    	System.out.println("real : setPrinterName()");
        this.name = name;
    }
    public String getPrinterName() {                // 이름의 취득
    	System.out.println("real : getPrinterName()");
        return name;
    }
    public void print(String string) {              // 이름을 붙여서 표시
        System.out.println("=== " + name + " ===");
        System.out.println(string);
    }
    private void heavyJob(String msg) {             // 무거운 작업
        System.out.print(msg);
        for (int i = 0; i < 5; i++) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
            }
            System.out.print(".");
        }
        System.out.println("완료");
    }
}
```

Main.java
```
public class Main {
    public static void main(String[] args) {
        Printable p = new PrinterProxy("Alice");
        System.out.println("현재의 이름은" + p.getPrinterName() + "입니다.");
        p.setPrinterName("Bob");
        System.out.println("현재의 이름은" + p.getPrinterName() + "입니다.");
        p.print("Hello, world.");
        
        p.print("test");
        p.setPrinterName("Tomas");
        System.out.println("현재의 이름은" + p.getPrinterName() + "입니다.");
        
    }
}
```
