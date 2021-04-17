# 어댑터 패턴

어댑터 패턴은 GoF 디자인 패턴 중 구조 패턴에 속하는 패턴으로 두 개의 다른 인터페이스 사이의 교량 역할을 할 때 사용합니다. 

<br>

아래와 같은 문제가 있을 때 사용을 고려할 수 있습니다.

* 어떤 소프트웨어 시스템이 존재하고 새로운 업체에서 제공한 클래스 라이브러리를 사용
* 새로운 업체에서 사용하는 인터페이스가 기존에 사용하는 인터페이스가 다르다고 가정
* 기존의 코드를 변경하여 문제를 해결 할 수 없는 상황
* 새로운 업체에서 제공 받은 클래스도 변경이 불가

이럴 때 업체에서 사용하는 인터페이스를 기존에 사용하던 인터페이스에 적응 시켜주는 클래스(어댑터)를 만들어 해결할 수 있습니다.

<br>

아래와 같은 장점이 있습니다.

* 관계가 없는 인터페이스 간에 공통된 사용법 적용이 가능합니다.
* 클래스의 재활용성이 증가합니다.

<br>

## Example

``` java
interface Duck {
    public void quack();
    public void fly();
}

interface Turkey {
    public void gobble();
    public void fly();
}
```

* 두 개의 다른 인터페이스를 구현합니다.

<br>

```java
class MallarDuck implements Duck {
    public void quack() {
        System.out.println("Quack");
    }
    
    public void fly() {
        System.out.println("I'm flying.");
    }
}

class WildTurkey implements Turkey {
    public void gobble() {
        System.out.println("Gobble gobble");
    }
    
    public void fly() {
        System.out.println("I'm flying a short distance.");
    }
}
```

* 각기 다른 인터페이스를 상속받은 클래스를 구현합니다.

<br>

``` java
class TurkeyAdapter implements Duck {
    private Turkey turkey;
    
    public TurkeyAdapter(Turkey turkey) {
        this.turkey = turkey;
    }
    
    public void quack() {
        turkey.gobble();
    }
    
    public void fly() {
        tureky.fly();
    }
}
```

* Duck 클래스의 메소드를 Turkey의 메소드 명으로 호출할 수 있도록 어댑터 클래스를 작성합니다.

<br>

``` java
public class AdapterPatternDemo {
    public static void main(String[] args) {
        MallerDuck duck = new MallerDuck();
        
        WildTurkey turkey = new WildTurkey();
        Duck turkeyAdapter = new TurekyApdater(turkey);
        
        duck.quack();
        duck.fly();
        
        System.out.println();
        
        turkeyAdapter.quack();
        turkeyAdapter.fly();
    }
}
```

```
// output
Quack
I'm flying.

Gobble
I'm flying a short distance.
```

* Duck의 메소드인 quack과 fly를 호출했음에도 Turkey의 gobble과 fly 메소드가 호출되었음을 확인할 수 있습니다.