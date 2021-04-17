# 컴퍼지트 패턴

컴퍼지트 패턴은 단일 객체(Single object)와 객체들의 집합(Group of objects)를 다루는 방법에 차이가 없을 경우 사용 가능한 패턴입니다. 트리구조를 생성할 때 사용하면 유용합니다. GoF 디자인 패턴 중 구조 패턴에 속합니다.

<br>

## Example

``` java
import java.util.ArrayList;
import java.util.List;

class Emplyee {
    private String name;
    private String dept;
    private int salary;
    private List<Employee> subordinates;
    
    public Employee(String name, String dept, int salary) {
        this.name = name;
        this.dept = dept;
        this.salary = salary;
    }
    
    public void add(Employee ) {
        subordinates.add(e);
    }
    
    public void remove(Employee e) {
        subordinates.remove(e);
    }
    
    public void printSubordinates() {
        System.out.println(this);
      	for(Employee e: subordinates)
            e.printSubordinates();
    }
    
    public List<Employee> getSubordinates() {
        return subordinates;
    }
    
    public String tostring() {
        return ("Employee :[ Name : " + name + ", dept : " + dept + ", salary :" + salary+" ]");
   }   
}
```

* 자기 자신을 원소로 갖는 리스트 객체를 필드로 갖고 있습니다.
* 해당 리스트에 대한 추가, 삭제, 반환 메서드를 작성합니다.

<br>

``` java
public class CompositePatternDemo {
    public static void main(String[] args) {
        Employee CEO = new Employee("John","CEO", 30000);

      Employee headSales = new Employee("Robert","Head Sales", 20000);

      Employee headMarketing = new Employee("Michel","Head Marketing", 20000);

      Employee clerk1 = new Employee("Laura","Marketing", 10000);
      Employee clerk2 = new Employee("Bob","Marketing", 10000);

      Employee salesExecutive1 = new Employee("Richard","Sales", 10000);
      Employee salesExecutive2 = new Employee("Rob","Sales", 10000);

      CEO.add(headSales);
      CEO.add(headMarketing);

      headSales.add(salesExecutive1);
      headSales.add(salesExecutive2);

      headMarketing.add(clerk1);
      headMarketing.add(clerk2);

      System.out.println(CEO); 
}
```

* Employee 객체를을 생성한 다음, 상관관계에 따라 트리 구조를 형성합니다.
* ceo와 ceo 아래에 위치한 직원들의 정보를 출력합니다.