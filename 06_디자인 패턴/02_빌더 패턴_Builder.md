# 빌더 패턴

빌더 패턴은 생성 패턴에 속하는 GoF 디자인 패턴으로 새로운 색체를 만들어서 반환하는 패턴입니다.
특징으로는 생성자가 들어갈 매개 변수의 수에 차례차례 매개 변수를 받아들여서 이 변수들을 통합해 한 번에 사용합니다.

<br>

아래는 사용해야 하는 경우의 예시입니다.

``` java
public class PersonInfo {
    private String name;
    private Integer age;
    private String color;
    private String animal;
    // 생략
    
    public PersonInfo(String name, Integer age, String color, String animal) {
        this.name = name;
        this.age = age;
        this.color = color;
        this.animal = animal;
    }
    
    // 필드의 getter 메서드 생략
}
```

위와 같이 클래스를 작성했을 때, 필드의 일부를 입력하거나 매개 변수로 입력하는 순서가 달라지면 문제가 발생할 수 있습니다.

<br>

## Example

위의 코드에서 고쳐야 할 부분은 다음과 같습니다.

* 불필요한 생성자를 만들지 않고 객체를 만들어야 합니다.
* 데이터의 순서에 상관없이 객체를 만들어야 합니다.
* 사용자가 봤을 때 명시적이고 이해하기 쉬워야 합니다.

<br>

아래의 코드는 위의 문제점을 빌더 패턴을 이용해 해결한 코드입니다.

``` java
public class PersonInfoBuilder {
    private String name;
    private Integer age;
    private String color;
    private String animal;
    
    public PersonInfoBuilder setName(String name) {
        this.name = name;
        return this;
    }
    
    public PersonInfoBuilder setAge(Integer age) {
        this.age = age;
        return this;
    }
    
    // color 와 animal 도 동일하게 setter 메서드를 작성
    
    // 다음은 빌더 패턴의 핵심 메서드
   	public PersonInfo build() {
        PersonInfo personInfo = new PersonInfo(name, age, color, animal);
        return personInfo;
    }
}
```

* PersonInfo 객체를 생성해주는 빌더 클래스를 별도로 작성했습니다.
* PersonInfo 와 동일한 필드를 가지고 있습니다.
* 각 필드의 setter 메서드는 자기자신(this)를 반환합니다.

<br>

사용 방법은 다음과 같습니다.

``` java
public static void main(String[] args) {
    PersonInfoBuilder personInfoBuilder = new PersonInfoBuilder();
    personInfo personInfo = persinInfoBuilder.setName("name")
        									.setAge(10)
        									.setColor("RED")
        									.setAnimal("Animal")
        									.build();
}
```

* 먼저 빌더 클래스의 객체를 생성합니다.
* 빌더 클래스의 setter 메서드를 연쇄적으로 호출한다음 build() 메서드로 객체를 반환합니다.

<br>

추가로 꼭 빌더 클래스와 만들 클래스를 분리하지 않아도 됩니다. 빌더 클래스를 만들 클래스의 내부 클래스로 작성하고 PersonInfo에 다음 메서드를 추가하면 됩니다.

``` java
public static PersonInfoBuilder Builder() {
    return new PersonInfoBuilder();
}
```

<br>

그러면 다음과 같이 사용이 가능합니다.

``` java
public static void main(String[] args) [
    PersonInfo personInfo = PersonInfo.Builder().setName("Name")....
]
```

