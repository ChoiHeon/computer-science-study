# 스트래티지 패턴

스트래티지 패턴은 행위를 클래스로 캡슐화하여 동적으로 행위를 사용자의 의도에 따라 변경이 가능한 구조입니다.  즉, 전략을 쉽게 변경할 수 있다는 의미입니다. GoF 디자인 패턴의 구조 패턴에 속합니다. 

<br>

## Example

``` java
interface Strategy {
    public int doOperation(int num1, int num2);
}
```

* 행위를 추상화 합니다.

<br>

``` java
class OeprationAdd implements Strategy {
    @Override
    public int doOperation(int num1, int num2) {
        return num1 + num2;
    }
}

class Operation Substract implements Strategy {
    @Override
    public int doOperation(int num1, int num2) {
        return num1 - num2;
    }
}

class OperationMultiply implements Strategy {
    @Override
    public int doOperation(int num1, int num2) {
        return num1 * num2;
    }
}
```

* 덧셈, 뺄셈, 곱셈을 의미하는 구체적인 행위를 작성합니다.

<br>

``` java
class Context {
    private Strategy strategy;
    
    public setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }
    
    public int executeStrategy(int num1, int num2) {
        System.out.println(strategy.doOperation(num1, num2));
    }
}
```

* Context 클래스에 Strategy에 대한 setter 메소드와 실행 메소드를 구현합니다.

<br>

```java
public class StrategyPatternDemo {
    public static void main(String[] args) {
    	Context context = new Context();
        
        context.setStrategy(new OperationAdd());
        context.executeStrategy(10, 20);
        
        context.setStrategy(new OperationSubstract());
        context.executeStrategy(10, 20);
        
        context.setStrategy(new OperationMultiply());
        context.executeStrategy(10, 20);
    }
}
```

``` 
// output
30
-10
200
```

* 필요에 따라 실행하는 행위를 변경합니다.

<br>

## 스트래티지 패턴 vs 브릿지 패턴

두 패턴은 실제로 UML에서 유사한 구조를 보이지만 목적 부분에서 차이점이 존재합니다.

* 스트래티지 패턴
  * 사용자가 사용하고 싶은 동작을 런타임 도중에, 적은 side-effects로 선택하여 진행할 수 있습니다.
  * HOW DO TOU BUILD A SOFTWARE COMPONENT?
* 브릿지 패턴
  * 추상화를 통해서 클래스와 인터페이스를 분리할 수 있습니다. 
  * HOW DO YOU WANT TO RUN A BEHAVIOUR IN SOFTWARE?