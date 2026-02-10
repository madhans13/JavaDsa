# Java Abstraction - Interview Preparation Guide

## Table of Contents
1. [What is Abstraction?](#what-is-abstraction)
2. [Abstract Classes](#abstract-classes)
3. [Interfaces](#interfaces)
4. [Abstract Class vs Interface](#abstract-class-vs-interface)
5. [Real-World Examples](#real-world-examples)
6. [Common Interview Questions](#common-interview-questions)
7. [Code Examples](#code-examples)
8. [Best Practices](#best-practices)

---

## What is Abstraction?

**Definition:** Abstraction is the process of hiding implementation details and showing only essential features/functionality to the user.

### Key Points:
- **Hides complexity** - User doesn't need to know internal working
- **Shows only essential features** - Exposes only what's necessary
- **Achieved through:** Abstract classes and Interfaces
- **Real-world analogy:** ATM machine - you use buttons, but don't know internal circuits

### Why Use Abstraction?
✅ Reduces code complexity  
✅ Increases code reusability  
✅ Improves maintainability  
✅ Enforces a contract/blueprint for subclasses  
✅ Achieves loose coupling  

---

## Abstract Classes

### Definition
An abstract class is a class declared with the `abstract` keyword that **cannot be instantiated** and may contain abstract methods.

### Syntax
```java
abstract class ClassName {
    // Abstract method (no body)
    abstract void methodName();
    
    // Concrete method (with body)
    void concreteMethod() {
        // implementation
    }
}
```

### Key Characteristics

| Feature | Details |
|---------|---------|
| **Instantiation** | Cannot create objects directly (`new Animal()` ❌) |
| **Abstract Methods** | Methods without body (must be implemented by subclass) |
| **Concrete Methods** | Can have normal methods with implementation |
| **Variables** | Can have instance variables and static variables |
| **Constructors** | Can have constructors |
| **Access Modifiers** | Can use any access modifier (public, private, protected) |
| **Inheritance** | Supports single inheritance only |
| **Keyword** | Uses `abstract` keyword |

### Rules for Abstract Classes

#### 1. Abstract Method Rules
- Abstract methods have **no body** (no curly braces)
- If a class has even **one abstract method**, the class **must be declared abstract**
- Abstract methods must be overridden in the subclass

```java
abstract class Animal {
    abstract void makeSound();  // No body - must end with semicolon
}
```

#### 2. Subclass Rules
- Subclass **must implement all abstract methods** OR be declared abstract itself
- Subclass can override concrete methods (optional)

```java
class Dog extends Animal {
    @Override
    void makeSound() {  // Must implement
        System.out.println("Bark!");
    }
}
```

#### 3. Constructor Rules
- Abstract class **can have constructors**
- Constructor is called when subclass object is created
- Used to initialize abstract class fields

```java
abstract class Vehicle {
    String brand;
    
    Vehicle(String brand) {  // Constructor
        this.brand = brand;
    }
    
    abstract void start();
}

class Car extends Vehicle {
    Car(String brand) {
        super(brand);  // Calls abstract class constructor
    }
    
    void start() {
        System.out.println(brand + " car started");
    }
}
```

### Complete Example
```java
abstract class BankAccount {
    protected String accountNumber;
    protected double balance;
    
    // Constructor
    public BankAccount(String accountNumber, double balance) {
        this.accountNumber = accountNumber;
        this.balance = balance;
    }
    
    // Abstract methods - must be implemented
    abstract void calculateInterest();
    abstract void withdraw(double amount);
    
    // Concrete method - common for all
    public void deposit(double amount) {
        balance += amount;
        System.out.println("Deposited: $" + amount);
    }
    
    // Concrete method
    public void displayBalance() {
        System.out.println("Balance: $" + balance);
    }
}

class SavingsAccount extends BankAccount {
    private double interestRate = 4.5;
    
    public SavingsAccount(String accountNumber, double balance) {
        super(accountNumber, balance);
    }
    
    @Override
    void calculateInterest() {
        double interest = balance * interestRate / 100;
        balance += interest;
        System.out.println("Interest: $" + interest);
    }
    
    @Override
    void withdraw(double amount) {
        if (balance - amount >= 1000) {
            balance -= amount;
            System.out.println("Withdrawn: $" + amount);
        } else {
            System.out.println("Minimum balance $1000 required!");
        }
    }
}
```

---

## Interfaces

### Definition
An interface is a **completely abstract** blueprint that defines a contract. Classes that implement an interface must provide implementation for all its methods.

### Syntax
```java
interface InterfaceName {
    // Abstract method (implicitly public and abstract)
    void methodName();
    
    // Default method (Java 8+)
    default void defaultMethod() {
        // implementation
    }
    
    // Static method (Java 8+)
    static void staticMethod() {
        // implementation
    }
}
```

### Key Characteristics

| Feature | Details |
|---------|---------|
| **Methods** | All methods are `public` and `abstract` by default (before Java 8) |
| **Variables** | All variables are `public`, `static`, and `final` (constants) |
| **Implementation** | Class uses `implements` keyword |
| **Multiple Inheritance** | A class can implement **multiple interfaces** |
| **Instantiation** | Cannot create objects directly |
| **Default Methods** | Can have method implementation (Java 8+) |
| **Static Methods** | Can have static methods (Java 8+) |
| **Private Methods** | Can have private methods (Java 9+) |

### Rules for Interfaces

#### 1. Method Rules (Pre-Java 8)
- All methods are **public** and **abstract** by default
- No need to write `public abstract` explicitly

```java
interface Drawable {
    void draw();  // Same as: public abstract void draw();
}
```

#### 2. Variable Rules
- All variables are **public**, **static**, and **final** by default
- Must be initialized when declared

```java
interface Constants {
    int MAX_USERS = 100;  // public static final int MAX_USERS = 100;
    String APP_NAME = "MyApp";
}
```

#### 3. Implementation Rules
- Class must implement **all methods** of the interface
- Use `implements` keyword
- Can implement **multiple interfaces**

```java
class Circle implements Drawable, Resizable {
    @Override
    public void draw() {  // Must be public
        System.out.println("Drawing circle");
    }
    
    @Override
    public void resize(int size) {
        System.out.println("Resizing to " + size);
    }
}
```

#### 4. Multiple Interface Implementation
```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }
    
    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
}
```

### Java 8+ Features

#### Default Methods
- Methods with implementation in interface
- Subclasses can override or use as-is

```java
interface Payment {
    void processPayment(double amount);
    
    // Default method
    default void printReceipt() {
        System.out.println("Payment receipt printed");
    }
}

class CreditCard implements Payment {
    @Override
    public void processPayment(double amount) {
        System.out.println("Paid $" + amount + " via Credit Card");
    }
    // printReceipt() inherited, no need to override
}
```

#### Static Methods
- Utility methods that belong to interface
- Called using interface name

```java
interface MathOperations {
    static int add(int a, int b) {
        return a + b;
    }
    
    static int multiply(int a, int b) {
        return a * b;
    }
}

// Usage
int result = MathOperations.add(5, 3);  // 8
```

### Complete Example
```java
interface Transferable {
    void transfer(BankAccount toAccount, double amount);
}

interface StatementProvider {
    void generateStatement();
    
    // Default method
    default void emailStatement(String email) {
        System.out.println("Statement sent to: " + email);
    }
}

interface OnlineBanking {
    void payBill(String biller, double amount);
    void setUpAutoDebit(String service, double amount);
}

class CurrentAccount implements Transferable, StatementProvider, OnlineBanking {
    private String accountNumber;
    private double balance;
    
    public CurrentAccount(String accountNumber, double balance) {
        this.accountNumber = accountNumber;
        this.balance = balance;
    }
    
    @Override
    public void transfer(BankAccount toAccount, double amount) {
        balance -= amount;
        System.out.println("Transferred: $" + amount);
    }
    
    @Override
    public void generateStatement() {
        System.out.println("Account: " + accountNumber);
        System.out.println("Balance: $" + balance);
    }
    
    @Override
    public void payBill(String biller, double amount) {
        balance -= amount;
        System.out.println("Paid $" + amount + " to " + biller);
    }
    
    @Override
    public void setUpAutoDebit(String service, double amount) {
        System.out.println("Auto-debit: " + service + " - $" + amount);
    }
}
```

---

## Abstract Class vs Interface

### Comparison Table

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| **Keyword** | `abstract` | `interface` |
| **Methods** | Can have abstract AND concrete methods | All abstract (before Java 8), can have default/static (Java 8+) |
| **Variables** | Can have any type of variables | Only `public static final` (constants) |
| **Access Modifiers** | Any (public, private, protected) | Only `public` |
| **Constructor** | Can have constructors | Cannot have constructors |
| **Inheritance** | Single inheritance (`extends`) | Multiple inheritance (`implements`) |
| **Speed** | Fast | Slightly slower (requires extra indirection) |
| **When to Use** | When classes share common behavior | When unrelated classes share common capabilities |
| **Represents** | "IS-A" relationship | "CAN-DO" relationship |

### When to Use What?

#### Use Abstract Class When:
✅ You want to share code among related classes  
✅ You need instance variables (non-static, non-final)  
✅ You want to provide common implementation  
✅ You need constructors to initialize fields  
✅ Classes are closely related  

**Example:** `Animal` → `Dog`, `Cat`, `Bird` (all ARE animals)

#### Use Interface When:
✅ You want unrelated classes to implement same methods  
✅ You want to specify behavior without implementation  
✅ You need multiple inheritance  
✅ You want to define capabilities/contracts  

**Example:** `Flyable` → `Bird`, `Airplane`, `Drone` (all CAN fly, but unrelated)

### Real-World Analogy

**Abstract Class:** 
- Think of **Vehicle** - all vehicles have engine, wheels, fuel
- Car, Bike, Truck all inherit common features

**Interface:**
- Think of **capabilities** - Flying, Swimming, Walking
- Bird can Fly and Walk, Fish can Swim, Duck can Fly and Swim
- Different animals, different capabilities

---

## Real-World Examples

### Example 1: E-Commerce Payment System

```java
// Abstract class for common payment features
abstract class Payment {
    protected String transactionId;
    protected double amount;
    
    public Payment(String transactionId, double amount) {
        this.transactionId = transactionId;
        this.amount = amount;
    }
    
    // Abstract method - each payment type processes differently
    abstract void processPayment();
    
    // Concrete method - common for all
    public void generateReceipt() {
        System.out.println("Receipt for transaction: " + transactionId);
        System.out.println("Amount: $" + amount);
    }
}

// Interface for refundable payments
interface Refundable {
    void initiateRefund(double amount);
}

// Interface for payments requiring verification
interface SecurePayment {
    boolean verifyOTP(String otp);
}

// Credit Card Payment
class CreditCardPayment extends Payment implements Refundable, SecurePayment {
    private String cardNumber;
    
    public CreditCardPayment(String transactionId, double amount, String cardNumber) {
        super(transactionId, amount);
        this.cardNumber = cardNumber;
    }
    
    @Override
    void processPayment() {
        System.out.println("Processing credit card payment: $" + amount);
        System.out.println("Card: ****" + cardNumber.substring(12));
    }
    
    @Override
    public void initiateRefund(double amount) {
        System.out.println("Refunding $" + amount + " to card");
    }
    
    @Override
    public boolean verifyOTP(String otp) {
        System.out.println("OTP verified: " + otp);
        return true;
    }
}

// UPI Payment
class UPIPayment extends Payment implements Refundable {
    private String upiId;
    
    public UPIPayment(String transactionId, double amount, String upiId) {
        super(transactionId, amount);
        this.upiId = upiId;
    }
    
    @Override
    void processPayment() {
        System.out.println("Processing UPI payment: $" + amount);
        System.out.println("UPI ID: " + upiId);
    }
    
    @Override
    public void initiateRefund(double amount) {
        System.out.println("Refunding $" + amount + " to UPI");
    }
}

// Cash on Delivery - No refund, no verification
class CashOnDelivery extends Payment {
    
    public CashOnDelivery(String transactionId, double amount) {
        super(transactionId, amount);
    }
    
    @Override
    void processPayment() {
        System.out.println("Cash on Delivery: $" + amount);
        System.out.println("Payment collected at doorstep");
    }
}
```

### Example 2: Employee Management System

```java
// Abstract class for common employee features
abstract class Employee {
    protected String empId;
    protected String name;
    protected double baseSalary;
    
    public Employee(String empId, String name, double baseSalary) {
        this.empId = empId;
        this.name = name;
        this.baseSalary = baseSalary;
    }
    
    // Abstract - different calculation for each type
    abstract double calculateSalary();
    
    // Concrete method
    public void displayInfo() {
        System.out.println("ID: " + empId);
        System.out.println("Name: " + name);
    }
}

// Interface for bonus eligible employees
interface BonusEligible {
    double calculateBonus(double performanceRating);
}

// Interface for employees with overtime
interface OvertimeEligible {
    double calculateOvertime(int hours);
}

// Permanent Employee
class PermanentEmployee extends Employee implements BonusEligible {
    private double hra;
    private double da;
    
    public PermanentEmployee(String empId, String name, double baseSalary, 
                            double hra, double da) {
        super(empId, name, baseSalary);
        this.hra = hra;
        this.da = da;
    }
    
    @Override
    double calculateSalary() {
        return baseSalary + hra + da;
    }
    
    @Override
    public double calculateBonus(double performanceRating) {
        return baseSalary * performanceRating / 100;
    }
}

// Contract Employee
class ContractEmployee extends Employee implements OvertimeEligible {
    private int contractMonths;
    
    public ContractEmployee(String empId, String name, double baseSalary, 
                           int contractMonths) {
        super(empId, name, baseSalary);
        this.contractMonths = contractMonths;
    }
    
    @Override
    double calculateSalary() {
        return baseSalary;  // Fixed salary
    }
    
    @Override
    public double calculateOvertime(int hours) {
        return hours * 20;  // $20 per hour
    }
}

// Intern - No bonus, No overtime
class Intern extends Employee {
    private int duration;
    
    public Intern(String empId, String name, double stipend, int duration) {
        super(empId, name, stipend);
        this.duration = duration;
    }
    
    @Override
    double calculateSalary() {
        return baseSalary;  // Stipend only
    }
}
```

---

## Common Interview Questions

### 1. Basic Conceptual Questions

**Q1: What is abstraction in Java?**
- Process of hiding implementation details and showing only functionality
- Achieved through abstract classes and interfaces
- Focuses on WHAT an object does, not HOW it does it

**Q2: Can we create an object of an abstract class?**
- No, abstract classes cannot be instantiated
- But we can create reference variables of abstract class type
```java
Animal animal = new Dog();  // ✅ Reference of abstract class
// Animal animal = new Animal();  // ❌ Cannot instantiate
```

**Q3: Can an abstract class have a constructor?**
- Yes, abstract classes can have constructors
- Called when subclass object is created
- Used to initialize abstract class fields

**Q4: Can an interface have a constructor?**
- No, interfaces cannot have constructors
- Because interfaces cannot be instantiated
- No instance variables to initialize

**Q5: Can abstract methods be static?**
- No, abstract methods cannot be static
- Abstract methods must be overridden, static methods cannot be overridden
- Static methods belong to class, not instance

**Q6: Can abstract methods be private?**
- No, abstract methods cannot be private
- Abstract methods must be accessible to subclass to override
- Private methods are not inherited

**Q7: Can we have an abstract class without any abstract methods?**
- Yes, it's valid
- Used when you don't want the class to be instantiated
- Example: Utility classes with only static methods

```java
abstract class Utility {
    public static void print(String msg) {
        System.out.println(msg);
    }
}
```

### 2. Interface-Specific Questions

**Q8: Can a class implement multiple interfaces?**
- Yes, Java supports multiple inheritance through interfaces
```java
class MyClass implements Interface1, Interface2, Interface3 { }
```

**Q9: What are default methods in interfaces? (Java 8)**
- Methods with implementation in interface
- Used to add new methods without breaking existing implementations
```java
interface MyInterface {
    default void newMethod() {
        System.out.println("Default implementation");
    }
}
```

**Q10: Can interfaces have variables?**
- Yes, but they are always `public static final` (constants)
- Must be initialized when declared
```java
interface Config {
    int MAX_SIZE = 100;  // public static final
}
```

**Q11: What is marker interface?**
- Interface with no methods
- Used to mark/tag classes for special behavior
- Examples: `Serializable`, `Cloneable`, `Remote`
```java
interface Deletable { }  // Marker interface

class User implements Deletable {
    // This class can be deleted
}
```

**Q12: Can an interface extend another interface?**
- Yes, interfaces can extend multiple interfaces
```java
interface A { void methodA(); }
interface B { void methodB(); }
interface C extends A, B { void methodC(); }
```

### 3. Comparison Questions

**Q13: Difference between abstract class and interface?**
Refer to comparison table above.

**Q14: When to use abstract class vs interface?**
- **Abstract class:** Related classes sharing common behavior
- **Interface:** Unrelated classes sharing common capabilities

**Q15: Can we have final methods in abstract class?**
- Yes, final methods can exist in abstract class
- Final methods cannot be overridden by subclass
```java
abstract class Parent {
    final void display() {  // Cannot override
        System.out.println("Final method");
    }
    abstract void show();  // Must override
}
```

### 4. Advanced Questions

**Q16: What is the diamond problem? How does Java solve it?**
- Problem: Class inherits from two classes with same method
- Java doesn't allow multiple inheritance of classes
- But allows multiple interface implementation
- If two interfaces have same default method, implementing class must override

```java
interface A {
    default void show() { System.out.println("A"); }
}

interface B {
    default void show() { System.out.println("B"); }
}

class C implements A, B {
    @Override
    public void show() {  // Must override to resolve conflict
        A.super.show();  // Can call specific interface method
    }
}
```

**Q17: Can we override static methods?**
- No, static methods cannot be overridden
- They can be hidden (method hiding, not overriding)
```java
class Parent {
    static void display() { System.out.println("Parent"); }
}

class Child extends Parent {
    static void display() { System.out.println("Child"); }  // Hiding, not overriding
}
```

**Q18: What is functional interface?**
- Interface with exactly one abstract method
- Used for lambda expressions (Java 8)
- Annotated with `@FunctionalInterface`
```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
    
    // Can have default and static methods
    default void print() { }
    static void show() { }
}

// Usage with lambda
Calculator add = (a, b) -> a + b;
System.out.println(add.calculate(5, 3));  // 8
```

**Q19: Can we declare interface as final?**
- No, interfaces cannot be final
- `final` means cannot be extended
- Interfaces are meant to be implemented

**Q20: What happens if a class implements two interfaces with same method signature?**
- No problem, just implement once
```java
interface A {
    void print();
}

interface B {
    void print();
}

class C implements A, B {
    @Override
    public void print() {  // Satisfies both A and B
        System.out.println("Implemented");
    }
}
```

---

## Code Examples

### Example 1: Shape Hierarchy

```java
// Abstract class
abstract class Shape {
    protected String color;
    
    public Shape(String color) {
        this.color = color;
    }
    
    abstract double calculateArea();
    abstract double calculatePerimeter();
    
    public void display() {
        System.out.println("Color: " + color);
        System.out.println("Area: " + calculateArea());
        System.out.println("Perimeter: " + calculatePerimeter());
    }
}

// Concrete class
class Circle extends Shape {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }
    
    @Override
    double calculateArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    double calculatePerimeter() {
        return 2 * Math.PI * radius;
    }
}

class Rectangle extends Shape {
    private double length;
    private double width;
    
    public Rectangle(String color, double length, double width) {
        super(color);
        this.length = length;
        this.width = width;
    }
    
    @Override
    double calculateArea() {
        return length * width;
    }
    
    @Override
    double calculatePerimeter() {
        return 2 * (length + width);
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        Shape circle = new Circle("Red", 5.0);
        Shape rectangle = new Rectangle("Blue", 4.0, 6.0);
        
        circle.display();
        System.out.println();
        rectangle.display();
    }
}
```

### Example 2: Vehicle System with Multiple Interfaces

```java
// Abstract class
abstract class Vehicle {
    protected String brand;
    protected String model;
    
    public Vehicle(String brand, String model) {
        this.brand = brand;
        this.model = model;
    }
    
    abstract void start();
    abstract void stop();
}

// Interfaces for capabilities
interface Electric {
    void chargeBattery();
    int getBatteryLevel();
}

interface GPS {
    void navigate(String destination);
}

interface SelfDriving {
    void enableAutoPilot();
    void disableAutoPilot();
}

// Electric Car
class TeslaCar extends Vehicle implements Electric, GPS, SelfDriving {
    private int batteryLevel;
    
    public TeslaCar(String brand, String model) {
        super(brand, model);
        this.batteryLevel = 100;
    }
    
    @Override
    void start() {
        System.out.println(brand + " " + model + " started silently");
    }
    
    @Override
    void stop() {
        System.out.println(brand + " " + model + " stopped");
    }
    
    @Override
    public void chargeBattery() {
        batteryLevel = 100;
        System.out.println("Battery fully charged");
    }
    
    @Override
    public int getBatteryLevel() {
        return batteryLevel;
    }
    
    @Override
    public void navigate(String destination) {
        System.out.println("Navigating to: " + destination);
    }
    
    @Override
    public void enableAutoPilot() {
        System.out.println("AutoPilot enabled");
    }
    
    @Override
    public void disableAutoPilot() {
        System.out.println("AutoPilot disabled");
    }
}

// Regular Car
class HondaCar extends Vehicle implements GPS {
    
    public HondaCar(String brand, String model) {
        super(brand, model);
    }
    
    @Override
    void start() {
        System.out.println(brand + " " + model + " engine started");
    }
    
    @Override
    void stop() {
        System.out.println(brand + " " + model + " engine stopped");
    }
    
    @Override
    public void navigate(String destination) {
        System.out.println("GPS navigating to: " + destination);
    }
}

// Usage
public class Main {
    public static void main(String[] args) {
        TeslaCar tesla = new TeslaCar("Tesla", "Model S");
        tesla.start();
        tesla.enableAutoPilot();
        tesla.navigate("San Francisco");
        System.out.println("Battery: " + tesla.getBatteryLevel() + "%");
        
        System.out.println();
        
        HondaCar honda = new HondaCar("Honda", "Civic");
        honda.start();
        honda.navigate("New York");
    }
}
```

---

## Best Practices

### 1. Naming Conventions
✅ Abstract classes: Use noun (e.g., `Animal`, `Vehicle`, `Employee`)  
✅ Interfaces: Use adjective ending with "-able" or "-ible" (e.g., `Runnable`, `Drawable`, `Serializable`)  
✅ Or capability names (e.g., `Payment`, `Logger`, `Validator`)

### 2. Design Principles
✅ Keep interfaces small and focused (Interface Segregation Principle)  
✅ Prefer interfaces over abstract classes for flexibility  
✅ Use abstract classes when you need to share code  
✅ Don't create interfaces with only one implementation  

### 3. Documentation
✅ Always document abstract methods explaining what subclass should implement  
✅ Document interface contracts clearly  
✅ Add JavaDoc comments for public APIs

### 4. When NOT to Use
❌ Don't use abstraction for simple classes  
❌ Don't create abstract classes just to prevent instantiation (use private constructor)  
❌ Don't create unnecessary interface hierarchies  

### 5. Common Mistakes to Avoid
❌ Forgetting to make methods `public` in interface implementation  
❌ Not implementing all abstract methods in subclass  
❌ Creating god interfaces (too many methods)  
❌ Using abstract class when interface would suffice  

---

## Quick Reference Card

```
┌─────────────────────────────────────────────────────────────┐
│                    ABSTRACTION CHEAT SHEET                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ABSTRACT CLASS                   INTERFACE                │
│  ───────────────                  ─────────                │
│                                                             │
│  abstract class Name { }          interface Name { }       │
│                                                             │
│  ✓ Abstract methods               ✓ Abstract methods       │
│  ✓ Concrete methods               ✓ Default methods (J8+)  │
│  ✓ Variables (any)                ✓ Constants only         │
│  ✓ Constructor                    ✗ No constructor         │
│  ✓ Access modifiers               ✓ Public only            │
│  ✗ Single inheritance             ✓ Multiple inheritance   │
│                                                             │
│  extends Keyword                  implements Keyword       │
│                                                             │
├─────────────────────────────────────────────────────────────┤
│                    WHEN TO USE WHAT?                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  USE ABSTRACT CLASS:              USE INTERFACE:           │
│  • Related classes                • Unrelated classes      │
│  • Share common code              • Define capabilities    │
│  • Need constructors              • Multiple inheritance   │
│  • IS-A relationship              • CAN-DO relationship    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Practice Questions for Interviews

### Quick Fire Questions (Be Ready to Answer)

1. What is abstraction?
2. Can you instantiate an abstract class?
3. Can abstract class have constructor?
4. Can interface have constructor?
5. Can we have main method in abstract class?
6. Difference between abstract class and interface?
7. What is marker interface? Give examples.
8. What is functional interface?
9. Can interface extend class?
10. Can class extend interface?
11. Can interface extend interface?
12. What are default methods?
13. Can we override default method?
14. What is diamond problem?
15. How many interfaces can a class implement?

### Coding Challenges (Be Ready to Write)

1. Create an abstract class `Shape` with abstract methods `area()` and `perimeter()`, implement for Circle and Rectangle
2. Create interfaces `Flyable`, `Swimmable` and implement for Duck class
3. Design a payment system using abstract class and interfaces
4. Implement a vehicle hierarchy with Electric and GPS capabilities
5. Create employee management system with different employee types

---

## Final Tips for Interviews

✅ **Understand concepts deeply** - Don't just memorize  
✅ **Practice coding** - Write actual code, don't just read  
✅ **Think real-world** - Relate to real applications  
✅ **Know the differences** - Abstract class vs Interface clearly  
✅ **Be ready to explain** - Why you chose abstract class over interface  
✅ **Know Java versions** - Features in Java 8, 9 onwards  
✅ **Design thinking** - Explain your design decisions  

---

**Good luck with your interview preparation! 🚀**