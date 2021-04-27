# 플라이웨이트 패턴

플라이웨이트 패턴은 생성되는 객체를 줄이고 메모리의 낭비를 줄여서 성능을 향상시키기 위한 패턴입니다. 객체를 줄이는 방법은 이미 존재하는 객체를 저장해서 재활용하는 것으로 원하는 객체를 찾을 수 없는 경우에만 새로 생성합니다.GoF 디자인 패턴 중 구조 패턴에 해당됩니다.

<br>

## Example

``` java
public interface Shape {
    void draw();
}
```

``` java
public class Circle implements Shape {
    private String color;
    private int x, y;
    private int radius;
    
    public Circle(String color) { this.color = color; }
    
    @Override
    public void draw() {
        System.out.println("Circle: Draw() [Color: " + color + "]");
   }
}
```

* 인터페이스 Shape를 정의한 다음, 원의 속성을 가지는 클래스 Circle를 작성합니다.

<br>

``` java
import java.util.HashMap;

public class ShapeFactory {
    private static final HashMap circleMap = new HashMap();
    
    public static Shape getCircle(String color) {
        Circle = circle = (Circle)circleMap.get(color);
        
        if (circle == null) {
            circle = new Circle(color);
            circleMap.push(color, Circle);
            System.out.println("Creating circle of color: " + color);
        }
        
        return circle;
    }
}
```

* 내부에 color와 Circle를 키, 값으로 갖는 HashMap 객체를 생성합니다.
* 지정한 color를 키 값으로 Circle 객체를 HashMap에서 탐색 및 반환합니다.
  * 없을 경우, 새로 생성하여 HashMap에 저장합니다.

<br>

``` java
public class FlyweightPatternDemo {
	public static void main(String[] args) {
        String colors[] = ["R", "G", "R", "B", "W", "G"];
        
        for (int i = 0; i < colors.length; i++) {
			Circle circle = (Circle)ShapeFactory.getCircle(color[i]);
            circle.draw();
            System.out.println();
        }
    }
}
```

```
// expected output
Creating circle of color: R
Circle: Draw() [Color: R]

Creating circle of color: G
Circle: Draw() [Color: G]

Circle: Draw() [Color: R]

Creating circle of color: B
Circle: Draw() [Color: B]

Creating circle of color: W
Circle: Draw() [Color: W]

Circle: Draw() [Color: G]
```

* 이미 생성한 color의 Circle의 객체는 새로 생성하지 않는 것을 확인할 수 있습니다.