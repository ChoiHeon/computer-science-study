# 책임 연쇄

책임 연쇄(Chain of Responsbility) 패턴은 클라이언트로부터 요청을 처리할 수 있는 처리객체를 만들어서 결합을 느슨하게 하기 위한 디자인 패턴입니다. 일반적으로 요청을 처리할 수 있는 객체를 찾을 떄까지 집합 안에서 요청을 전달하는 역할을 합니다.

<br>

다음과 같은 상황에 적용하면 유용한 패턴입니다.

- 요청의 발신자와 수신자를 분리해야 하는 경우
- 요청을 처리할 수 있는 객체가 여러개일 때, 그 중 하나에 요청을 보내려는 경우
- 코드에서 처리객체(handler)를 명시적을 지정하고 싶지 않은 경우

<br>

## 장단점

* 장점
  * 결합도를 낮추어 요청의 발신자와 수신자를 분리시킵니다.
  * 클라이언트는 처리객체의 집합 내부의 구조를 알 필요가 없습니다.
  * 집합 내의 처리 순서를 변경하거나 처리객체를 추가 또는 삭제할 수 있는 유연성이 향상됩니다.
* 단점
  * 충분한 디버깅을 거치지 않을 경우, 집합 내부에서 사이클이 발생할 수 있습니다.
  * 디버깅 및 테스트가 쉽지 않습니다.

<br>

## Example

```java
public abstract class AbstractLogger {
    public static int INFO = 1;
    public static int DEBUG = 2;
    public static int ERROR = 3;
    
    protected int level;
    
    // next element in chain or responsibility
    protected AbstractLogger nextLogger;
    
    public void  setNextLogger(AbstractLogger nextLooger) {
        this.nextLogger = nextLogger;
    }
    
    public void logMessage(int level, String message) {
        if (this.level <= level)
            write(message);
        if (nextLogger != null)
            nextLogger.logMessage(level, message);
    }
    
    abstract protected void write(String message);
}
```

* Handler 클래스입니다.
* 상위 레벨에서 호출했을 경우 write 메서드를 호출합니다.
* 연결된 Handler (nextLogger)가 있을 경우, 같은 메서드를 실행합니다.

<br>

``` java
public class ConsoleLogger extends AbstractLogger {
    public Consolelogger(int level) {
        this.level = level;
    }
    
    @Override
    protected void write(String message) {
        System.out.println("Standard Console::Logger: " + message)
    }
}
```

```java
public class ErrorLogger extends AbstractLogger {
    public ErrorLogger(int level) {
        this.level = level;
    }
    
    @Override
    protected void write(String message) {
        System.out.println("Error Console::Loger: " + message);
    }
}
```

``` java
public class FileLogger extends AbstractLogger {
    public FileLogger(int level) {
        this.level = level;
    }
    
    @Override
    protected void write(String mesage) {
        System.out.println("File::Logger: " + message);
    }
}
```

* ConcreteHandler 클래스를 작성합니다.
* 객체 생성시 level을 인자로 받고 write 메서드를 구체화합니다.

<br>

```java
public class ChainPatternDemo {
    private static AbstractLogger getChainOfLoggers() {
        AbstractLogger errorLogger = new ErrorLogger(AbstractLogger.ERROR);
    	AbstractLogger fileLogger = new FileLogger(AbstractLogger.DEBUG);
        AbstractLogger consoleLogger = new ConsoleLogger(AbstractLogger.INFO);

        errorLogger.setNextLogger(fileLogger);
        fileLogger.setNextLogger(consoleLogger);

        return errorLogger;	
    }
    
    public static void main(String[] args) {
        AbstractLogger loggerChain = getChainLoggers();
        
        loggerChain.logMessage(AbstractLogger.INFO, 
         "This is an information.");
		System.out.println()
        
        loggerChain.logMessage(AbstractLogger.DEBUG, 
         "This is an debug level information.");
		System.out.println()
        
        loggerChain.logMessage(AbstractLogger.ERROR, 
         "This is an error information.");
    }
}
```

```
// expected output
Standard Console::Logger: This is an information.

File::Logger: This is an debug level information.
Standard Console::Logger: This is an debug level information.

Error Console::Logger: This is an error information.
File::Logger: This is an error information.
Standard Console::Logger: This is an error information.
```



* getChainOfLoggers()
  * ConcreteHandler 객체들을 생성합니다.
  * 각 객체의 nextLogger를 저장합니다.
  * 가장 level 필드가 높은 객체(errorLogger)를 반환합니다.
* main()
  * getChainLoggers 메서드를 통해서 ConcreteHandler 객체를 생성, 반환받습니다.
  * 원하는 level과 message를 인자로 넣은 logMessage를 실행합니다.
* output
  * 연결된 객체들에 대해, 인자로 넣은 레벨보다 낮을 경우 메세지를 출력합니다. 

