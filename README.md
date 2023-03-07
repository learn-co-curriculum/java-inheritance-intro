# Inheritance

## Learning Goals

- Demonstrate the problem of code duplication
- Demonstrate a solution using inheritance

## Introduction

Another object-oriented programming pillar is inheritance, which is the
ability for one class to inherit from another class. We can think
of inheritance as a hierarchical classification where a class
can inherit another class' features (fields and methods).
In this lesson, we will explore how to use inheritance in Java.

## The Problem with Code Duplication

Before we implement an application with inheritance, we'll see an
example that does not use inheritance and instead relies
on code duplication.  Code duplication can result in maintenance issues
when the code needs to change.

Let's assume our application needs to keep inventory of the items stocked in a grocery store.
Every grocery item has an id, name, and price.  Some grocery items are perishable,
so they have an expiration date in addition to the id, name, and price.

Our initial model is shown in the UML
class diagram below.   `PerishableItem` has the same fields
and methods as `GroceryItem`, along with an additional field `expiration`
and methods `getExpiration()` and `setExpiration()` (highlighted in yellow).
Everything not highlighted in yellow represents duplication between the two classes.

![class diagram version1](https://curriculum-content.s3.amazonaws.com/6677/pillars/classdiagram1.png)

The Java implementation for `GroceryItem` and `PerishableItem` are shown
below.  Notice it takes some effort in looking at the code to figure out
what is similar and what is different between the two classes.

```java
public class GroceryItem {
    private String id;
    private String name;
    private double price;

    public String getId() {return id;}
    public void setId(String id) {this.id = id;}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    public double getPrice() {return price;}
    public void setPrice(double price) {this.price = price;}
}
```

```java
public class PerishableItem {
    private String id;
    private String name;
    private double price;
    private String expiration;   // additional field

    public String getId() {return id;}
    public void setId(String id) {this.id = id;}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    public double getPrice() {return price;}
    public void setPrice(double price) {this.price = price;}
    public String getExpiration() {return expiration;}    // additional method
    public void setExpiration(String expiration) {this.expiration = expiration;}   //additional method
}
```

Let's implement a driver class `Main` to create an instance of each class.

```java
public class Main {
    public static void main(String[] args) {
        GroceryItem item1 = new GroceryItem();
        item1.setId("ABCD-1234");
        item1.setName("1 Roll Paper Towel");
        item1.setPrice(0.99);

        PerishableItem item2 = new PerishableItem();
        item2.setId("PQRS-5555");
        item2.setName("Cantaloupe Melon");
        item2.setPrice(3.19);
        item2.setExpiration("06/09/23");  

        System.out.println("Items created");
    }
}
```

We can set a breakpoint and use the Java visualizer to look at the state of each object:


![object state version1](https://curriculum-content.s3.amazonaws.com/6677/pillars/objectstate1.png)

The problem with code duplication is that we now have code that is difficult
to maintain.  Let's say we decide to change the name of the `id` field to `sku`.
The abbreviation `sku` stands for "stock keeping unit", which is a common term used by retailers
to identify an inventory item.

Since `GroceryItem` and `PerishableItem` have a field named `id` and methods
`getId()` and `setId()`, we need to update both classes.  Imagine if we had even
more types of grocery item classes that needed updating (`AgeRestrictedItems`, `HealthAndBeautyItems`, etc).
Unfortunately, it is all too common to forget to update all necessary code when there is duplication.

## An Inheritance Solution

Thankfully, Java is an object-oriented language that supports **inheritance**.
Inheritance lets us establish a relationship between two classes that allows
the fields and methods of one class (called the **super** or **parent** or **base** class)
to be inherited by another class (called the **sub** or **child** or **derived** class).

![inheritance diagram](https://curriculum-content.s3.amazonaws.com/6677/pillars/supersub.png)

- The superclass declares the fields and methods that are common to both classes.
- The subclass inherits the fields and methods of the superclass.
- The subclass can declare additional fields and methods.


In the UML class diagram above, each instance of `Subclass` has two fields
(`field2` and the inherited `field1`) and can be used to call two
methods (`method2()` and the inherited method `method1()`).

Let's consider how to use inheritance to implement the relationship
between `GroceryItem` and `PerishableItem`.

![class diagram version2](https://curriculum-content.s3.amazonaws.com/6677/pillars/classdiagram2.png)

- `GroceryItem` is the superclass containing the fields `id`, `name`, and `price`,
  along associated methods for the fields.
- `PerishableItem` is the subclass containing the field `expiration` and associated methods.


The `GroceryItem` class looks the same in Java:

```java
public class GroceryItem {
    private String id;
    private String name;
    private double price;

    public String getId() {return id;}
    public void setId(String id) {this.id = id;}
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    public double getPrice() {return price;}
    public void setPrice(double price) {this.price = price;}
}
```

However, the `PerishableItem` class will use inheritance to avoid
duplicating the fields and methods from `GroceryItem`.
We establish the inheritance relationship in a Java subclass
using the keyword `extends`  as shown below:

```java
public class PerishableItem extends GroceryItem {
  private String expiration;

  public String getExpiration() {return expiration;}
  public void setExpiration(String expiration) {this.expiration = expiration;}
}
```

Notice that `PerishableItem` simply declares it extends `GroceryItem`, and
then adds the `expiration` instance variable and the `getExpiration()` and
`setExpiration()` methods.  

- Each `PerishableItem` class instance will contain both the inherited
  fields `id`, `name`, and `price` along with the additional field `expiration`.
- We can call inherited methods on a `PerishableItem` class instance.

The `Main` class remains the same:

```java
public class Main {

    public static void main(String[] args) {
        GroceryItem item1 = new GroceryItem();
        item1.setId("ABCD-1234");
        item1.setName("1 Roll Paper Towel");
        item1.setPrice(0.99);

        PerishableItem item2 = new PerishableItem();
        item2.setId("PQRS-5555");
        item2.setName("Cantaloupe Melon");
        item2.setPrice(3.19);
        item2.setExpiration("06/09/23");

        System.out.println("Items created");
    }
}
```

If we debug the application, we see the same object state since `PerishableItem`
inherits the fields from `GroceryItem`:

![object state version1](https://curriculum-content.s3.amazonaws.com/6677/pillars/objectstate1.png)


## Evolving the super class

![class diagram version 3](https://curriculum-content.s3.amazonaws.com/6677/pillars/classdiagram3.png)

Now we can easily change `id` to `sku` in the `GroceryItem` superclass.

```java
public class GroceryItem {
    private String sku;                               // update field name
    private String name;
    private double price;

    public String getSku() {return sku;}              // update method name
    public void setSku(String sku) {this.sku = sku;}  // update method name
    public String getName() {return name;}
    public void setName(String name) {this.name = name;}
    public double getPrice() {return price;}
    public void setPrice(double price) {this.price = price;}
}
```

No changes are necessary for the subclass `PerishableItem`, since
it will inherit the updated field and methods.


```java
public class PerishableItem extends GroceryItem {
    private String expiration;

    public String getExpiration() {
        return expiration;
    }

    public void setExpiration(String expiration) {
        this.expiration = expiration;
    }
}
```

The `Main` driver class needs to be updated to call the `setSku()` method,
as the method name was changed in the superclass:

```java
public class Main {

    public static void main(String[] args) {
        GroceryItem item1 = new GroceryItem();
        item1.setSku("ABCD-1234");              // update method call
        item1.setName("1 Roll Paper Towel");
        item1.setPrice(0.99);

        PerishableItem item2 = new PerishableItem();
        item2.setSku("PQRS-5555");              // update method call
        item2.setName("Cantaloupe Melon");
        item2.setPrice(3.19);
        item2.setExpiration("06/09/23");

        System.out.println("Items created");
    }
}
```

We can use the Java visualizer to check that both objects have the `sku` field
rather than `id`:

![object state version 3](https://curriculum-content.s3.amazonaws.com/6677/pillars/objectstate3.png)



## Conclusion

Inheritance is an important and powerful concept in object-oriented programming.
The key to understanding inheritance is that it provides code re-usability. 
Instead of duplicating fields and methods in multiple classes,
we can simply inherit the members of one class (superclass) into another class (subclass).

By promoting code reuse and reducing code redundancy, inheritance
makes it easier to adapt and extend a class.

## Resources

- [The Java Tutorials - Inheritance](https://docs.oracle.com/javase/tutorial/java/IandI/subclasses.html)
