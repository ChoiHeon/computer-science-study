# 싱글톤 패턴

싱글톤 패턴은 GoF 디자인 패턴 중 생성 패턴에 속하는 패턴으로 유일하게 존재해야 하는 객체를 다루는 방법입니다. 멀티 스레드에서 특정 클래스의 객체가 2개 이상이면 안 될 경우에 주로 사용합니다.

<br>

## Example

싱글톤은 자기자신을 내부 객체로 저장함으로서 필요시에 내부 객체를 반환하는 패턴입니다. 이 때 내부 객체를 생성하는 방법이나 반환하는 방법에 따라 여러가지로 나뉠 수 있습니다.

<br>

1. Eager Initialization

```java
class Singleton {
	private static Singleton instance = new Singleton();
    
    public static Singleton getInstance() {
        return instance;
    }
}
```

* 클래스 로드 시 객체를 생성하기 때문에 메모리 누수가 발생할 수 있습니다.

<br>

2. Thread safe Lazy initialization

``` java
class Singleton {
    private static Singleton instance = null;
    
    public static synchronized Singleton getInstance() {
        if (instance == null)
            intance = new Singleton();
        return instance;
    }
}
```

* getInstance() 메서드 호출시 내부 객체의 값이 null이라면 생성자를 호출한 뒤 반환합니다.
  * 필요할 때 객체를 생성하므로 메모리의 누수를 방지할 수 있습니다.
* 멀티 스레드 환경에서 하나의 스레드만 getInstance() 메서드를 호출할 수 있습니다.
  * 스레드를 lock, unlock 해야 하므로 이에 대해 비용이 많이 발생할 수 있습니다.

<br>

3. Thread safe Lazy initialization + Double-checked locking (사용권장 X)

``` java
class Singleton {
	private static Singleton instance;
    
    public static Singleton getInstance() {
        if (intance == null) {
            synchronized (Singlecon.class) {
                if instance == null) 
                    instance = new Singleton();
            }
        }
        
        return instance;
    }
}
```

* Thread safe Lazy Initialization의 getInstance() 메서드는 최초 호출 이후에 불필요하게 동기화를 하기 때문에 Double-checked locking 기법을 추가했습니다.
* 이론상으론 완벽하지만,  실제로는자바 플랫폼의 메모리 모델 때문에 작동을 보장하지 못합니다.
* **따라서 사용하지 말아야 하는 방식입니다.**

<br>

4. **Initialization on demand holder idiom**

``` java
class Singleton {
    private static class SingletonHolder {
        private static final Singleton instance = new Singleton();
    }
    
    public static Singleton getInstance() {
        return SingletonHolder.instance;
    }
}
```

* Singleton 내부에 SingletonHolder 클래스의 객체가 없기 때문에 클래스 로드 단계에서 instance를 초기화 하지 않습니다.
* final을 사용해 다시 값이 할당되지 않도록 합니다.
* Singleton 객체 생성의 위험이 있다면 사용하지 말아야 합니다.
* 리플렉션으로 인해 내부 생성자가 호출될 가능성이 있습니다.
* 역직렬화 수행 시 새로운 객체를 생성합니다.
* **성능 향상에 유용합니다.**

<br>

5. **Enum initialization**

``` java
public enum Singleton {
    INSTANCE;
    
    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

* enum은 프로그램 내 한 번만 초기화하기 때문에 싱글톤 패턴 구현에 유용합니다.
* 리플렉션에 안전합니다.
* 직렬화를 보장합니다.

* 컴파일 단계에서 초기화가 결정되므로 메소드를 호출할 때마다 Context 정보를 넘겨야 합니다.

* **안정성이 보장됩니다.**