# 커맨드 패턴

커맨드(Command) 패턴은 행위(Behavioral) 패턴 중 하나로 실행될 기능을 캡슐화함으로써 주어진 여러 기능을 실행할 수 있는, 재사용성이 높은 클래스를 설계하는 패턴입니다.

따라서 이벤트가 발생했을 때 실행될 기능이 다양하면서도 변경이 필요한 경우, 이벤트를 발생시키는 클래스를 변경하지 않고 재사용할 때 유용합니다.

<br>

## Example

```java
public interface Order {
    void execute();
}
```

```java
public class Stock {
    private String name = "ABC";
    private int quentity = 10;
    
    public void buy() {
        System.out.println("Stock [ Name: ]" + name + 
                          ", Quantity: " + quantity + 
                           "] bought");
    }
    
    public void sell() {
        System.out.println("Stock [ Name: ]" + name + 
                          ", Quantity: " + quantity + 
                           "] sold");
    }
}
```

```java
public class BuyStock implements Order{
    private Stock abcStock;
    
    public BuyStock(Stock abcStock) {
        this.abcStock = abcStock;
    }
    
    public void execute() {
        abcStock.buy();
    }
}

public class SellStock implements Order {
    private Stock abcStock;
    
    public SellStock(Stock abcStock) {
        this.abcStock = abcStock;
    }
    
    public void execute() {
        abcStock.sell();
    }
}
```

```java
import java.util.ArrayList;
import java.util.List;

public class Broker {
    public private List<Order> orderList = new ArrayList<Order>();
    
    public void takeOrder(Order order) {
        orderList.add(order);
    }
    
    public void placeOrders() {
        for(Order order : orderList) 
            order.execute();
        
        orderList.clear();
    }
}
```

```java
public class CommandPatternDemo {
    public static void main(String[] args) {
        Stock abcStock = new Stock();
        
        BuyStock buyStockOrder = new BuyStock(abcStock);
        SellStock sellStockOrder = new SellStock(abcStock);
        
        Broker broker = new Broker();
        broker.takeOrder(buyStockOrder);
        broker.takeOrder(sellStockOrder);
        
        broker.placeOrders();
    }
}
```

```
// output
Stock [ Name: ABC, Quantity: 10 ] bought
Stock [ Name: ABC, Quantity: 10 ] sold
```

