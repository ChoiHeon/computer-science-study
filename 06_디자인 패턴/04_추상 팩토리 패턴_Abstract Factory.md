# 추상 팩토리 패턴

추상 팩토리 패턴은 생성 패턴에 속하는 GoF 디자인 패턴으로, 많은 수의 서브 클래스를 특정 그룹으로 묶어 한 번에 교체할 수 있는 디자인 패턴입니다. 추상 팩토리 패턴의 인터페이스는 클래스를 명시하지 않고 연관도니 객체의 팩토리를 생성하는데 도움을 줍니다. 직접 객체를 생성하는 클래스는 팩토리 메서드 패턴이 적용되어 있습니다.

<br>

## Example

``` java
interface Shape {
    void draw();
}

class RoudedRectangle implements Shape {
    @Override
    public voi draw() {
        System.out.println("Inside RoundedRectangle::draw()");
    }
}

class RoudedSquare implements Shape {
    @Override
    public voi draw() {
        System.out.println("Inside RoundedRectangle::draw()");
    }
}

class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Inside Rectangle::draw()");
    }
}

class Square implements Shape {
    @Overiide
    public void draw() {
        System.out.println("Inside Square::draw()");
    }
}
```

* Shape 인터페이스에서 파생된 RoundedRectangle, Rectangle 클래스를 작성합니다.

<br>

``` java
abstract class AbstractFactory {
    abstract Shape getShape(String shapeType);
}

class ShapeFactory extends AbstractFactory {
    @Override
    public Shape getShape(String shapeType) {
        if (shapeType.equalsIgnoreCase("RECTANGLE"))
			return new Rectangle;
        else if (shapeType.equalsIgnoreCase("SQUARE"))
            return new Square();
        return null;
    }
}

class RoundedShapeFactory extends AbstractFactory {
    @Override
    public Shape getShape(String shapeType) {
        if (shapeType.equalsIgnoreCase("ROUNDEDRECTANGLE"))
            return new RoundedRectangle;
        else if (shapeType.equalsIgnoreCase("ROUNDEDSQUARE"))
            return new RoundedSquare();
        return null;
    }
}
```

* 팩토리 메서드 패턴이 적용된 팩토리 클래스와 팩토리 클래스의 추상 클래스를 작성합니다.

<br>

``` java
class FactoryProducer {
    public static AbstractFactory getFactory(boolean rounded) {
        if (rounded)
            return new RoundedShapeFactory();
        else
            return new ShapeFactory();
    }
}
```

* 팩토리 메서드를 반환하는 일종의 팩토리 클래스를 작성합니다.

<br>

``` java
public class AbstactFactoryPetternDemo {
    public static void main(String[] args) {
        AbstractFactory shapeFactory = FactoryProducer.getFactory(false);
        Shape shape1 = shapeFactory.getShape("SQUARE");
        
        AbstractFactory roundedShapeFactory = FactoryProducer.getFactory(true);
        shape shape2 = roundedShapeFactory.getShape("SQUARE");
        
        shape1.draw();
        shape2.draw();
    }
}
```

``` 
// output
Inside Square::draw()
Inside RoundedSquare::draw()
```

* FactoryProducer 클래스를 통해 팩토리 클래스를 선택합니다.
* 선택한 팩토리 클래스에 따라 정해진 객체를 생성합니다.
* getShape 메서드에 같은 인자를 넣었음에도 다른 결과를 출력하는 것을 확인할 수 있습니다.