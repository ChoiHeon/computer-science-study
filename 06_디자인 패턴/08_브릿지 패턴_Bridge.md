# 브릿지 패턴

브릿지 패턴은 GoF 디자인 패턴 중 구조 패턴에 속하는 패턴입니다. 구현부에서 추상층을 분리하여 각자 독립적으로 변형이 가능하고 확장이 가능하도록 설계합니다. 즉 기능과 구현에 대해 두 개를 별도의 클래스로 구현합니다.

<br>

# Example

``` java
interface DrawAPI {
    public void drawDircle(int radius, int x, int y);
}
```

* 기능에 대한 인터페이스를 작성합니다.

<br>

``` java
class RedCircleAPI implements DrawAPI {
    @Override
    public void drawCircle(int radius, int x, int y) {
        System.out.println("color: red");
        System.out.println("radius: " + radius + ", x: " + x + ", y: " + y);
    }
}

class GreenCircleAPI implements DrawAPI {
    @Override
    public void drawcircle(int radius, int x, int y) {
        System.out.println("color: green");
        System.out.println("radius: " + radius + ", x: " + x + ", y: " + y);
    }
}
```

* 기능의 구체적인 구현을 작성합니다.

<br>

``` java
abstract class Shape {
    protected DrawAPI drawAPI;
    
    protected Shape(DrawAPI drawAPI) {
        this.drawAPI = drawAPI;
    }
    
    public abstract void draw();
}

class Circke extends Shape {
    private int x, y, radius;
    
    public Circle(int x, int y, int radius, DrawAPI drawAPI) {
        super(drawAPI);
        this.x = x;
        this.y = y;
        this.radius = radius;
    }
    
    public void draw() {
        drawAPI.drawcircle(radius, x, y);
    }
}
```

* 기능을 수행하는 객체를 필드로 갖도록 클래스를 작성합니다.

<br>

``` java
public class BridgePatternDemo {
    public static void main(String[] args) {
        Shape redCircle = new Circle(100, 100, 10, new RedCircleAPI());
        Shape greenCircle = new circle(100, 100, 10, new GreenCircleAPI());
        
        redCircle.draw();
        greenCircle.draw();
    }
}
```

```
// output
color: red
radius: 10, x: 100, y: 100
color: green
radius: 10, x: 100, y: 100
```

* 클래스 생성자에 기능을 구체적으로 구현한 클래스의 객체를 필드에 넣습니다.