# 퍼사드 패턴

퍼사트 패턴은 클래스 라이브러리 같은 어떤 소프트웨어의 다른 커다란 코드 부분에 대해 간략화된 인터페이스를 제공하는 패턴입니다. 사용자가 외관(Facade)만을 알면 사용이 가능하도록 하는 것이 목표입니다.

어댑터 패턴과 유사한데, 쉽고 단순한 인터페이스를 사용하고 이용하고 싶다면 퍼사드 패턴을 사용하고 인터페이스를 다른 인터페이스로 변환하기 위한 용도로 사용됩니다.

<br>

## Example

```java
public interface Shape {
    void draw();
}
```

```java
public class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Rectangle::draw()");
    }
}
```

```java
public class Square implements Shape {
    @Override
    public void draw() {
        System.out.println("Square::draw()");
    }
}
```

```java
public class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Cicle::draw()");
    }
}
```

<br>

```java
public class ShapeMaker {
    private Shape circle;
    private Shape rectangle;
    private Shape square;
    
    public ShapeMaker() {
        circle = new Circle();
        rectangle = new Rectangle();
        square = new Square();
    }
    
    public void drawCircle() {
        circle.draw();
    }
    
    public void drawRectangle() {
        rectangle.draw();
    }
    
    public void drawSquare() {
        square.draw();
    }
}
```

* 인터페이스 Shape를 구현한 각 클래스의 객체를 필드로 생성합니다.
* 각 객체의 메서드를 호출하는 메서드를 작성합니다.

<br>

``` java
public class FacadePatternDemo {
    public static void main(String[] args) {
        ShapeMaker shapeMaker = new ShapeMaker();
        
        shapeMaker.drawCircle();
        shapeMaker.drawRectangle();
        shapeMaker.drawSquare();
    }
}
```

```
// output
Circle::draw()
Rectangle::draw()
Square::draw()
```

* 퍼사드 객체 shapeMaker를 생성해서 인터페이스 Shape의 객체를 단순화 시켰습니다.
* 이는 클라이언트와 구성요소들로 이루어진 서브시스템을 분리시키는 역할도 합니다. 여기선 draw 기능을 분리하는데 사용되었습니다.

