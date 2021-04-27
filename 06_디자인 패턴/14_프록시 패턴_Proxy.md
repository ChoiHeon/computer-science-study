# 프록시 패턴

프록시 패턴은 클래스가 다른 클래스의 기능을 대신 수행해주는 패턴입니다. 즉 실제 클래스 대신 가상 클래스를 설계하여 **흐름을 제어**하는 목적으로 사용됩니다. 이렇게 하면 다음과 같은 장점이 있습니다.

* 원래 기능을 수행하며 부가적인 작업을 수행하기 좋습니다.
* 비용이 많이 드는 연산을 실제로 필요한 시점에 수행할 수 있습니다.
* 설계 원칙인 OCP, DIP를 잘 지킬 수 있습니다.

<br>

## Example

```java
public interface Image {
    void display();
}
```

```java
public class RealImage implements Image {
    
    private String fileName;
    
    public RealImage(String fileName) {
        this.fileName = fileName;
    }
    
    @Override
    public void display() {
        System.out.println("Displaying " + fileName);
    }
}
```

* Image 인터페이스를 구체화한 RealImage 클래스를 생성합니다.

<br>

```java
public class ProxyImage implements Image {
    
    private Image image;
    private String fileName;
    
    public ProxyImage(String fileName) {
        this.fileName = fileName;
    }
    
    @Override
    public void display() {
        if (image == null) {
			image = new RealImage(fileName)
        	loadFromDisk()
        }
        
      	image.display();
    }
    
    private void loadFromDisk() {
        System.out.println("Loading " + fileName);
    }
}
```

* Image 인터페이스를 구체화한 객체를 필드로 갖습니다.
* 필드의 메서드와 동일한 이름인 display 메서드를 가집니다.
  * display 메서드 호출시 필요에 따라 부가적인 작업이 수행됩니다. (loadFromDisk 메서드)

<br>

```java
public class ProxyPatternDemo {
    
    public static void main(String[] args) {
        Image image = new ProxyIamge("test_image.jpg");
        
        image.display();
        System.out.println();
        image.display();
    }
}
```

```
// expected output

Loading: test_image.jpg
Diplaying: test_image.jpg

Diplaying: test_image.jpg
```

* 첫 번째 display 메서드 호출시 loadFromDisk 메서드가 실행됩니다.

  * 호출하기 전엔 image 필드는 필요없으므로 초기화되지 않았습니다.

    