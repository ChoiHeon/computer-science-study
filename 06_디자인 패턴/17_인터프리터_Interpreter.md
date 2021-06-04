# 인터프리터 패턴

인터프리터(Interpreter) 패턴은 행위(Behavioral) 패턴 중 하나로 문법을 표현하는 방법 중 하나입니다. SQL parsing이나 Symbol processing engin 등에 사용됩니다. 인터프리터 패턴을 사용하면 문법의 추가 및 수정, 구현이 쉬워집니다. 다만 복잡한 문법의 경우 관리 및 유지하기 어려울 수 있습니다.

<br>

## Exmaple

```java
public interface Expression [
    public boolean interpret(String context);
]
```

```java
public class TerminalExpression implements Expression {
    private String data;
    
    public TerminalExpression(String data) {
        this.data = data;
    }
    
    @Override
    public boolean interpret(String context) {
        if (context.contains(data))
            return true;
        return false;
    }
}

public class OrExpression implements Expression {
    private Expression exp1 = null;
    private Expression exp2 = null;
    
    pu9blic OrExpression(Expression exp1, Expression exp2) {
        this.exp1 = exp1;
        this.exp2 = exp2;
    }
    
    @Override
    public boolean interpert(String context) {
        return exp1,interpret(context) || exp1.interpret(context);
    }
}

public class AndExpression implements Expression {
    private Expression exp1 = null;
    private Expression exp2 = null;
    
    public AndExpression(Expression exp1, Expression exp2) {
        this.exp1 = exp1;
        this.exp2 = exp2;
    }
    
    @Override
    @Override
    public boolean interpert(String context) {
        return exp1,interpret(context) && exp1.interpret(context);
    } 
}
```

```java
public class InterperterPatternDemo {
    
    // Rule: Robert and John are male
    public static Expression getMaleExpression() {
        Expression robert = new TerminalExpression("Robert");
        Expression john = new TerminalExpression("John");
        return new OrExpression(robert, john);
    }
    
    // Rule: Jolie is a married waman
    public static Expression getMarriedWomanExpressio() {
        Expression julie = new TerminalExpression("Julie");
        Expression married = new TerminalExpression("Married");
        return new AndExpression(julie, married);
    }

	public static void main(String[] args) {
        Expression isMail = getMaleExpression();
        Expression isMarriedWoman = getMarriedWomanExpression();
        
        System.out.println("John is male? " + isMale.interpret("John"));
      	System.out.println("Julie is a married women? " + isMarriedWoman.interpret("Married Julie"));
   }
}
```



