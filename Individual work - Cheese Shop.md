**MAIN**    

```java
              import java.util.Iterator;
              import java.util.List;
              import java.util.ArrayList;
              import java.util.Scanner;

              public class Main {
                  public static void main(String[] args) {
                      CheeseService cheeseService = new CheeseService();

                      Cheese cheddar = new Cheese("Cheddar", 6.69);
                      Cheese mozzarella = new Cheese("Mozzarella", 3.50);
                      Cheese brie = new Cheese("Brie", 7.77);
                      Cheese feta = new Cheese("Feta", 4.20);
                      Cheese ricotta = new Cheese("Ricotta", 4.21);
                      Cheese blueCheese = new Cheese("Blue Cheese", 9.99);

                      cheeseService.addCheese(cheddar);
                      cheeseService.addCheese(mozzarella);
                      cheeseService.addCheese(brie);
                      cheeseService.addCheese(feta);
                      cheeseService.addCheese(ricotta);
                      cheeseService.addCheese(blueCheese);

                      CheeseShop shop = new CheeseShop(cheeseService);

        
                      shop.addCheeseToCart(cheddar);
                      shop.addCheeseToCart(mozzarella);
                      shop.addCheeseToCart(brie);
                      shop.addCheeseToCart(feta);
                      shop.addCheeseToCart(ricotta);
                      shop.addCheeseToCart(blueCheese);

                      Customer customer = new Customer(50.0); 

                      System.out.printf("Customer's initial balance: $%.2f%n", customer.getMoney());

                    
                      List<Cheese> cartCopy = new ArrayList<>(shop.getCart());

                  
                      Iterator<Cheese> iterator = cartCopy.iterator();
                      while (iterator.hasNext()) {
                          Cheese cheese = iterator.next();
                          customer.buyCheese(shop, cheese);
                      }

                      System.out.printf("Customer's remaining balance: $%.2f%n", customer.getMoney());

                      
                      List<Cheese> ownedItems = customer.getOwnedItems();
                      List<String> cheeseNames = new ArrayList<>();
                      for (Cheese cheese : ownedItems) {
                          cheeseNames.add(cheese.getName());
                      }
                      System.out.println("Cheeses bought: " + cheeseNames);

                      Scanner scanner = new Scanner(System.in);
                      System.out.print("Enter the name of the cheese to remove: ");
                      String cheeseToRemove = scanner.nextLine();

                      customer.removeCheese(cheeseToRemove);
                      System.out.println("Cheeses bought after removal: " + customer.getOwnedItems());

                      scanner.close();
                  }
              }
```
**CHEESESHOP**

```java

import java.util.ArrayList;
import java.util.List;

public class CheeseShop {
    private CheeseService cheeseService;
    private List<Cheese> cart;

    public CheeseShop(CheeseService cheeseService) {
        this.cheeseService = cheeseService;
        this.cart = new ArrayList<>();
    }

    public void addCheeseToCart(Cheese cheese) {
        cart.add(cheese);
    }

    public void removeCheeseFromCart(Cheese cheese) {
        cart.remove(cheese);
    }

    public List<Cheese> getCart() {
        return cart;
    }

    public List<Cheese> getAvailableCheeses() {
        return cheeseService.getAvailableCheeses();
    }
}
```

**CHEESE**

```java

public class Cheese {
    private String name;
    private double price;

    public Cheese(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }

    @Override
    public String toString() {
        return name + " - $" + price;
    }
}
```

**CHEESESERVICE**

```java

import java.util.ArrayList;
import java.util.List;

public class CheeseService {
    private List<Cheese> availableCheeses;

    public CheeseService() {
        this.availableCheeses = new ArrayList<>();
    }

    public void addCheese(Cheese cheese) {
        availableCheeses.add(cheese);
    }

    public List<Cheese> getAvailableCheeses() {
        return availableCheeses;
    }
}
```

**CUSTOMER**

```java 

import java.util.ArrayList;
import java.util.List;

public class Customer {
    private double money;
    private List<Cheese> ownedItems;

    public Customer(double money) {
        this.money = money;
        this.ownedItems = new ArrayList<>();
    }

    public void buyCheese(CheeseShop shop, Cheese cheese) {
        if (money >= cheese.getPrice()) {
            /
            money -= cheese.getPrice();
            ownedItems.add(cheese);
           
            shop.removeCheeseFromCart(cheese);
            System.out.println("Successfully purchased " + cheese.getName());
        } else {
            System.out.println("Not enough money to buy " + cheese.getName());
        }
    }

    public void removeCheese(String cheeseName) {
        for (int i = 0; i < ownedItems.size(); i++) {
            Cheese cheese = ownedItems.get(i);
            if (cheese.getName().equalsIgnoreCase(cheeseName)) {
                ownedItems.remove(i);
                System.out.println("Removed " + cheeseName + " from owned items.");
                return;
            }
        }
        System.out.println("Could not find " + cheeseName + " in owned items.");
    }

    public double getMoney() {
        return money;
    }

    public List<Cheese> getOwnedItems() {
        return ownedItems;
    }
}
```