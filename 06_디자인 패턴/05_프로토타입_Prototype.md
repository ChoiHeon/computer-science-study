# 프로토타입 패턴

프로토타입 패턴은 GoF 디자인 패턴의 생성 패턴으로 생성할 객체를 생성할 때 이미 존재하는 객체를 복제하고 데이터의 일부를 수정하는 패턴입니다. 객체를 DB에 1회 접근하거나 새로 생성하는 비용보다 복제하는 비용이 더 적기 때문에 유용합니다.

<br>

## Example

```java
abstract class Shape implements Cloneable {
    private String type;
    
    abstract void draw();
    
    public String getType() {
        return type;
    }
    
    public void setType() {
        this.type = type;
    }
    
    @Override
    public Object clone() throws CloneNotSupportedException {
        Object clone = super.clone();
        return clone;
    }
}
```

* 추상 클래스에 필드와 추상 메서드 그리고 복제 메서드를 구현합니다.

<br>

``` java
class  Circle extends Shape {
    public Circle() {
        type="Circle";
    }
    
    @Override
    public void draw() {
        System.out.println("Inside Circle::draw()");
    }
}

class  Rectangle extends Shape {
    public Rectangle() {
        type="Rectangle";
    }
    
    @Override
    public void draw() {
        System.out.println("Inside Rectangle::draw()");
    }
}

class  Square extends Shape {
    public Square() {
        type="Square";
    }
    
    @Override
    public void draw() {
        System.out.println("Inside Square::draw()");
    }
}
```

* Shape를 상속받는 클래스 Circle, Rectangle, Square를 구현합니다.

<br>

``` java
public class PrototypePatternDemo {
    public static void main(String[] args) {
        Shape circle = new Circle();
        
        Shape rectancgle = (Shape)circle.clone();
        rectangle.setType("Rectangle");
        
        Shape square = (Shape)rectangle.clone();
        square.setType("Square");
    }
}
```

* 처음 생성한 객체를 복제하여 나머지 두 개 객체를 만들었습니다.

<br>

외에도 클래스를 데이터베이스에서 가져와 구체화하고, 해쉬테이블에 저장하는 클래스를 작성해 비용을 줄이는 방법이 있습니다.

``` java
import java.util.Hashtable;

class ShapedCache {
    private static Hashtable<String, Shape> shapeMap = new Hashtable<String, Shape>();
    
    public static Shape getShape(String shapeType) {
        Shape cachedShape = shapeMap.get(shapeType);
        return (Shape)cachedShape.clone();
    }
    
    public static void loadCache() {
		shapeMap.put("Circle", new Circle());
        shapeMap.put("Rectangle", new Rectangle());
        shapeMap.put("Square", new Square());
    }
}

public class PrototypePatternDemo {
    public static void main(String[] args) {
        ShapeCache.loadShape();
        
        Shape circle = ShapeCache.getShape("Circle");
        Shape rectancgle = ShapeCache.getShape("Rectangle");
        Shape square = ShapeCache.getShape("Square");
    }
}
```

* getShape()는 미리 생성해둔 객체를 복제해서 반환합니다. 
* loadCache()는 미리 객체를 생성한 뒤 해쉬테이블에 저장합니다.