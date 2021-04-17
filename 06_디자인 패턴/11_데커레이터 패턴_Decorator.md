# 데커레이터 패턴

데커레이터 패턴은 객체를 동적으로 서브 클래스를 이용해 확장하는 패턴입니다. 여기서 동적이란 런타임 도중에도 가능하다는 것을 의미합니다. 객체의 확장은 기본 기능에 추가할 수 있는 기능을 데커레이터 패턴으로 구현한 뒤 조합을 통해서 설계하는 방식입니다. GoF 디자인 패턴의 구조 패턴에 속합니다.

<br>

## Example

``` java
interface interface Shape {
    void draw();
}
```

``` java
class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Shape Circle");
    }
}

```

* 인터페이스와 기본 기능을 구현한 클래스를 작성합니다.

<br>

``` java
abstract class ShapeDecorator implements Shape {
    protected Shape decoratedShape;
    
    public ShapeDecorator(Shape decoratedShape) {
        this.decoratedShape = decoratedShape;
    }
    
    public void draw() {
        decoratedShape.draw();
    }
}
```

* 추가 기능을 구현할 추상 클래스를 작성합니다. 이 추상 클래스는 기본 기능 인터페이스를 상속받습니다.
* 상속 받은 기본 기능 인터페이스 타입의 객체를 생성자의 인자로 받은 객체로 저장합니다.
* 기본 기능 인터페이스에 있는 메소드와 같은 이름의 메소드인 draw를 가져야 합니다.
  * 동일한 방법으로 사용하기 위해서 입니다.

<br>

``` java
class RedShapeDecorator extends ShapeDecorator {
    public RedShapeDecorator(Shape decoratedShape) {
        super(decoratedShape);
    }
    
    @Override
    public void draw() {
        decoratedShape.draw();
        System.out.println("Border Color: Red");
    }
}

class ThickShapeDecorator extends ShapeDecorator {
    public ThickShapeDecorator(Shape decoratedShape) {
        super(decoratedShape);
    }
    
    @Override
    public void draw() {
        decoratedShape.draw();
        System.out.println("Border size: 100px")
    }
}
```

* 두 가지 추가 기능을 구현합니다.
* 추가 기능은 연쇄적으로 추가가 가능하기 위해서 생성자에서 상위 클래스의 생성자(super)를 호출합니다.

<br>

``` java
class DecoratorPatternDemo {
    public static void main(String[] args) {
        Shape circle = new Circle();
        Shape redCircle = new RedShapeDecorator(circle);
        Shape thickRedCircle = new ThickShapeDecorator(redCircle);
        
        circle.draw();
        redCircle.draw();
        redRectangle.draw();
    }
}
```

```
// output
Shape: Circle			// circle.draw()

Shape: Circle			// redCircle.draw()
Border Color: Red		

Shape: Circle			// thickRedCircle.draw()
Border Color: Red
Border Size: 100px
```

* 단계적으로 기능이 추가되었음을 확인할 수 있습니다.

