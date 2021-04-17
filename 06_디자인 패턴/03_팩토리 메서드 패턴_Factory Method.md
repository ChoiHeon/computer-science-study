# 팩토리 메서드 패턴

팩토리 메서드 패턴은 생성 패턴에 속하는 GoF 디자인 패턴으로 생성 로직을 노출하지 않고 객체를 생성할 수 있습니다. 팩토리 패턴이라고도 합니다.

<br>

## Example

다음은 Shape 인터페이스에 대해 Rectangle, Square, Circle 클래스가 파생되었을 때 팩토리 메서드 패턴을 통해서 공통된 방법으로 위의 클래스들의 객체를 생성할 수 있는 방법입니다.

<br>

``` java
interface Shape {
    void draw();
}

class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Inside Rectangle:;draw()");
    }
}

class Square implements Shape {
    @Overiide
    public void draw() {
        System.out.println("Inside Square::draw()");
    }
}

class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Inside Circle::draw()");
    }
}
```

* Shape 인터페이스의  draw 메서드를 각 서브 클래스가 오버라이드 했습니다.

<br>

``` java
class ShapeFactory {
    public Shape getShape(String shapeType) {
        if (shapeType )
            return null;
        if (shapeType.equalsIgnoreCase("CIRCLE"))
            return new Circle();
        else if (shapeType.equalsIgnoreCase("RECTANGLE"))
            return new Rectangle();
        else if (shapeType.equalsIgnoreCase("SQUARE"))
            return new Square();
        
        return null;
    }
}

public class FactoryPatternDemo {
    public static void main(String[] args) {
        ShapeFactory shapeFactory = new ShapeFactory();
        
        Shape shape1 = shapeFactory.getShape("CIRCLE");
        Shape shape2 = shapeFactory.getShape("RECTANGLE");
        Shape shape3 = shapeFactory.getShape("SQUARE");
        
        shape1.draw();
        shape2.draw();
        shape3.draw();
    }
}
```

```
// Output
Inside Circle:;draw()
Inside Rectangle:;draw()
Inside Square:;draw()
```

* Shape 인터페이스에서 파생된 클래스의 객체를 생성해주는 팩토리 클래스 ShapeFactory를 작성합니다.
  * 인자로 받은 shapeType의 값에 따라서 별도의 객체를 반환합니다.
* shape1, shape2, shape3는 각각 다른 결과를 출력합니다.

