# Object-Oriented Programming (OOP) Concepts 🚀

A comprehensive guide to understanding Object-Oriented Programming concepts with real-world examples and interview preparation notes.

---

## Table of Contents
1. [Introduction](#introduction)
2. [Core OOP Concepts](#core-oop-concepts)
   - [Class and Object](#1-class-and-object)
   - [Encapsulation](#2-encapsulation)
   - [Inheritance](#3-inheritance)
   - [Polymorphism](#4-polymorphism)
   - [Abstraction](#5-abstraction)
3. [Interfaces](#interfaces)
4. [Abstract Classes](#abstract-classes)
5. [Types of Inheritance](#types-of-inheritance)
6. [Real-World Examples](#real-world-examples)
7. [Interview Tips](#interview-tips)

---

## Introduction

Object-Oriented Programming (OOP) is a programming paradigm based on the concept of "objects" which contain data and code. The four main pillars of OOP are:
- **Encapsulation**
- **Inheritance**
- **Polymorphism**
- **Abstraction**

---

## Core OOP Concepts

### 1. Class and Object

**Definition:**
- **Class**: A blueprint or template for creating objects
- **Object**: An instance of a class with actual values

**Code Example:**
```java
class Car {
    String color;
    String brand;
    String model;
    
    void start() {
        System.out.println("Car started");
    }
    
    void stop() {
        System.out.println("Car stopped");
    }
}

// Creating objects
Car myCar = new Car();
myCar.color = "Red";
myCar.brand = "Toyota";
```

**Real-World Example: Netflix User Accounts**
```java
class NetflixUser {
    String username;
    String email;
    String subscriptionPlan; // Basic, Standard, Premium
    List<String> watchHistory;
    
    void playMovie(String movieName) {
        System.out.println("Playing: " + movieName);
    }
    
    void addToWatchlist(String movieName) {
        System.out.println("Added to watchlist");
    }
}

NetflixUser user1 = new NetflixUser();
user1.username = "john_doe";
user1.subscriptionPlan = "Premium";
```

**💡 Key Point:** Netflix User is a class (blueprint), John and Jane are objects (actual users).

---

### 2. Encapsulation

**Definition:**
Bundling data (variables) and methods that operate on that data within a single unit (class), and hiding internal details using access modifiers.

**Why Use Private Variables with Public Methods?**
- **Control and Validation**: Add validation logic
- **Read-Only/Write-Only Access**: Control access permissions
- **Future Flexibility**: Change internal implementation without breaking code
- **Logging and Debugging**: Add monitoring capabilities

**Code Example:**
```java
class BankAccount {
    private double balance; // Hidden from outside
    
    public void deposit(double amount) {
        if(amount > 0) {
            balance += amount;
            System.out.println("Deposited: " + amount);
        }
    }
    
    public double getBalance() {
        return balance;
    }
    
    public void withdraw(double amount) {
        if(amount > 0 && amount <= balance) {
            balance -= amount;
        } else {
            System.out.println("Insufficient balance");
        }
    }
}
```

**Real-World Example: ATM System**
```java
class ATM {
    private double cashAvailable;
    private String atmLocation;
    
    public boolean withdrawCash(double amount, String pin) {
        if(validatePin(pin) && amount <= cashAvailable) {
            cashAvailable -= amount;
            System.out.println("Cash withdrawn: " + amount);
            return true;
        }
        return false;
    }
    
    public double checkBalance(String pin) {
        if(validatePin(pin)) {
            return cashAvailable;
        }
        return 0;
    }
    
    private boolean validatePin(String pin) {
        // Internal validation logic
        return true;
    }
}
```

**💡 Key Point:** You can't directly access cash in ATM. You must use withdraw/deposit methods with proper authentication.

---

### 3. Inheritance

**Definition:**
A mechanism where a new class inherits properties and behaviors from an existing class, promoting code reusability.

**Code Example:**
```java
class Vehicle {
    String brand;
    int speed;
    
    void move() {
        System.out.println("Vehicle is moving");
    }
}

class Car extends Vehicle {
    int numberOfDoors;
    
    void playMusic() {
        System.out.println("Playing music");
    }
}

class Bike extends Vehicle {
    boolean hasCarrier;
    
    void wheelie() {
        System.out.println("Doing a wheelie!");
    }
}
```

**Real-World Example: Social Media Posts**
```java
class Post {
    String author;
    String content;
    int likes;
    List<String> comments;
    
    void like() {
        likes++;
    }
    
    void addComment(String comment) {
        comments.add(comment);
    }
}

class PhotoPost extends Post {
    String imageUrl;
    String filter;
    
    void applyFilter(String filterName) {
        this.filter = filterName;
    }
}

class VideoPost extends Post {
    String videoUrl;
    int duration;
    
    void play() {
        System.out.println("Playing video...");
    }
}
```

**💡 Key Point:** All Instagram posts (photo/video/story) have likes and comments, but each type has unique features.

---

### 4. Polymorphism

**Definition:**
The ability of objects to take multiple forms. Same method name behaving differently based on the object.

**Types:**
- **Compile-time (Method Overloading)**: Same method name, different parameters
- **Runtime (Method Overriding)**: Child class provides specific implementation

**Code Example:**
```java
class Calculator {
    // Method Overloading
    int add(int a, int b) {
        return a + b;
    }
    
    double add(double a, double b) {
        return a + b;
    }
    
    int add(int a, int b, int c) {
        return a + b + c;
    }
}

// Method Overriding
class Animal {
    void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    void makeSound() {
        System.out.println("Dog barks: Woof Woof!");
    }
}

class Cat extends Animal {
    void makeSound() {
        System.out.println("Cat meows: Meow Meow!");
    }
}
```

**Real-World Example: Food Delivery System**
```java
interface DeliveryPartner {
    void deliver(Order order);
}

class BikeDelivery implements DeliveryPartner {
    public void deliver(Order order) {
        System.out.println("Delivering by bike - ETA: 20-30 mins");
    }
}

class CarDelivery implements DeliveryPartner {
    public void deliver(Order order) {
        System.out.println("Delivering by car - ETA: 30-40 mins");
    }
}

class WalkingDelivery implements DeliveryPartner {
    public void deliver(Order order) {
        System.out.println("Delivering by walking - ETA: 10-15 mins");
    }
}

// Main application
class FoodDeliveryApp {
    public void processDelivery(DeliveryPartner partner, Order order) {
        partner.deliver(order); // Same method, different behavior
    }
}
```

**💡 Key Point:** Swiggy/Zomato calls deliver() method, but behavior changes based on delivery partner type.

---

### 5. Abstraction

**Definition:**
Hiding complex implementation details and showing only essential features to the user.

**Code Example:**
```java
abstract class Shape {
    abstract double calculateArea();
    abstract double calculatePerimeter();
    
    void display() {
        System.out.println("This is a shape");
    }
}

class Circle extends Shape {
    double radius;
    
    double calculateArea() {
        return Math.PI * radius * radius;
    }
    
    double calculatePerimeter() {
        return 2 * Math.PI * radius;
    }
}

class Rectangle extends Shape {
    double length, width;
    
    double calculateArea() {
        return length * width;
    }
    
    double calculatePerimeter() {
        return 2 * (length + width);
    }
}
```

**Real-World Example: Payment Gateway**
```java
abstract class PaymentGateway {
    String merchantId;
    
    abstract boolean authenticate();
    abstract boolean processTransaction(double amount);
    abstract void sendConfirmation();
    
    public final boolean makePayment(double amount) {
        if(authenticate()) {
            if(processTransaction(amount)) {
                sendConfirmation();
                return true;
            }
        }
        return false;
    }
}

class RazorpayGateway extends PaymentGateway {
    boolean authenticate() {
        // Complex authentication logic hidden
        return true;
    }
    
    boolean processTransaction(double amount) {
        // Razorpay API calls hidden
        return true;
    }
    
    void sendConfirmation() {
        System.out.println("Razorpay: Payment confirmed");
    }
}

// E-commerce checkout
class Checkout {
    public void processPayment(PaymentGateway gateway, double amount) {
        gateway.makePayment(amount); // Simple interface, complex logic hidden
    }
}
```

**💡 Key Point:** When you pay on Amazon, you don't see authentication, encryption, or API calls - just a simple pay button.

---

## Interfaces

**Definition:**
A contract that defines what a class should do, but not how it should do it. Provides 100% abstraction.

**Key Features:**
- All methods are `public abstract` by default (before Java 8)
- All variables are `public static final` (constants)
- A class can implement multiple interfaces
- From Java 8: Can have `default` and `static` methods
- From Java 9: Can have `private` methods

**Code Example:**
```java
interface Vehicle {
    void start();
    void stop();
    int MAX_SPEED = 120; // public static final
}

class Car implements Vehicle {
    public void start() {
        System.out.println("Car started with key");
    }
    
    public void stop() {
        System.out.println("Car stopped");
    }
}

class Bike implements Vehicle {
    public void start() {
        System.out.println("Bike started with kick");
    }
    
    public void stop() {
        System.out.println("Bike stopped");
    }
}
```

**Multiple Inheritance with Interfaces:**
```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    public void fly() {
        System.out.println("Duck is flying");
    }
    
    public void swim() {
        System.out.println("Duck is swimming");
    }
}
```

**Real-World Example: Smart Home Devices**
```java
interface VoiceControllable {
    void respondToVoice(String command);
}

class SmartLight implements VoiceControllable {
    public void respondToVoice(String command) {
        if(command.contains("turn on")) {
            System.out.println("Light turned ON");
        }
    }
}

class SmartAC implements VoiceControllable {
    public void respondToVoice(String command) {
        if(command.contains("temperature")) {
            System.out.println("AC temperature adjusted");
        }
    }
}

// Alexa/Google Home can control any VoiceControllable device
class VoiceAssistant {
    public void controlDevice(VoiceControllable device, String command) {
        device.respondToVoice(command);
    }
}
```

**💡 Key Point:** Alexa controls lights, AC, TV through a common interface - all respond to voice commands.

---

## Abstract Classes

**Definition:**
A class that cannot be instantiated and may contain both abstract methods (without body) and concrete methods (with body).

**When to Use:**
- When you have some common implementation to share
- When classes share a relationship (IS-A)
- When you want to provide default behavior

**Code Example:**
```java
abstract class BankAccount {
    protected double balance;
    String accountNumber;
    
    // Abstract method
    abstract void calculateInterest();
    
    // Concrete method
    public void deposit(double amount) {
        balance += amount;
        System.out.println("Deposited: " + amount);
    }
    
    public void withdraw(double amount) {
        if(amount <= balance) {
            balance -= amount;
        }
    }
}

class SavingsAccount extends BankAccount {
    void calculateInterest() {
        balance += balance * 0.04; // 4% interest
    }
}

class CurrentAccount extends BankAccount {
    void calculateInterest() {
        balance += balance * 0.01; // 1% interest
    }
}
```

**Interface vs Abstract Class:**

| Feature | Interface | Abstract Class |
|---------|-----------|----------------|
| Methods | Only abstract (before Java 8) | Both abstract and concrete |
| Variables | Only constants (public static final) | Any type of variables |
| Constructor | Cannot have | Can have |
| Inheritance | Multiple inheritance supported | Single inheritance only |
| When to use | Define contract for unrelated classes | Share common code among related classes |

---

## Types of Inheritance

### 1. Single Inheritance
One class inherits from one parent class.

```java
class Employee {
    String name;
    double salary;
    
    void work() {
        System.out.println("Employee is working");
    }
}

class Manager extends Employee {
    int teamSize;
    
    void conductMeeting() {
        System.out.println("Manager conducting meeting");
    }
}
```

**Real-World:** Manager IS-A Employee with additional responsibilities.

---

### 2. Multilevel Inheritance
A chain of inheritance (A → B → C).

```java
class Vehicle {
    String registrationNumber;
    
    void start() {
        System.out.println("Vehicle started");
    }
}

class MotorVehicle extends Vehicle {
    String engineType;
    
    void refuel() {
        System.out.println("Refueling...");
    }
}

class Car extends MotorVehicle {
    int numberOfDoors;
    
    void openTrunk() {
        System.out.println("Trunk opened");
    }
}
```

**Real-World:** Car → Motor Vehicle → Vehicle (each level adds specificity).

---

### 3. Hierarchical Inheritance
Multiple classes inherit from a single parent.

```java
class BankAccount {
    String accountNumber;
    double balance;
    
    void deposit(double amount) {
        balance += amount;
    }
}

class SavingsAccount extends BankAccount {
    double interestRate = 4.0;
    
    void calculateInterest() {
        balance += balance * interestRate / 100;
    }
}

class CurrentAccount extends BankAccount {
    double overdraftLimit = 50000;
    
    void allowOverdraft() {
        System.out.println("Overdraft allowed up to: " + overdraftLimit);
    }
}

class FixedDepositAccount extends BankAccount {
    int lockInPeriod = 5;
}
```

**Real-World:** Different bank account types - all are accounts but with different features.

---

### 4. Multiple Inheritance (Through Interfaces)
A class inherits from multiple sources.

```java
interface Camera {
    void capturePhoto();
}

interface GPS {
    void getLocation();
}

interface MusicPlayer {
    void playMusic();
}

class Smartphone implements Camera, GPS, MusicPlayer {
    public void capturePhoto() {
        System.out.println("Photo captured");
    }
    
    public void getLocation() {
        System.out.println("Current location retrieved");
    }
    
    public void playMusic() {
        System.out.println("Playing music");
    }
}
```

**Real-World:** Your smartphone has camera, GPS, music player capabilities from different sources.

**Why Java Doesn't Support Multiple Class Inheritance:**
```java
// Diamond Problem - Not allowed in Java
class A {
    void display() { System.out.println("A"); }
}

class B extends A {
    void display() { System.out.println("B"); }
}

class C extends A {
    void display() { System.out.println("C"); }
}

// Which display() should D inherit? - Ambiguity!
// class D extends B, C { } // ❌ Compilation Error
```

---

### 5. Hybrid Inheritance
Combination of two or more types of inheritance.

```java
class WearableDevice {
    String brand;
    double batteryLife;
    
    void chargeBattery() {
        System.out.println("Charging...");
    }
}

interface HealthMonitor {
    void trackHeartRate();
    void countSteps();
}

interface NotificationDevice {
    void showNotifications();
}

class SmartWatch extends WearableDevice 
    implements HealthMonitor, NotificationDevice {
    
    public void trackHeartRate() {
        System.out.println("Tracking heart rate");
    }
    
    public void countSteps() {
        System.out.println("Counting steps");
    }
    
    public void showNotifications() {
        System.out.println("Showing notifications");
    }
}
```

**Real-World:** Apple Watch/Fitbit - inherits from WearableDevice + implements multiple interfaces.

---

## Real-World Examples

### Example 1: E-Commerce System

```java
// Abstraction with interfaces
interface PaymentMethod {
    boolean processPayment(double amount);
    void refund(double amount);
}

// Multiple implementations - Polymorphism
class CreditCardPayment implements PaymentMethod {
    public boolean processPayment(double amount) {
        System.out.println("Processing credit card payment: $" + amount);
        return true;
    }
    
    public void refund(double amount) {
        System.out.println("Refunding to credit card: $" + amount);
    }
}

class UPIPayment implements PaymentMethod {
    public boolean processPayment(double amount) {
        System.out.println("Processing UPI payment: ₹" + amount);
        return true;
    }
    
    public void refund(double amount) {
        System.out.println("Refunding to UPI: ₹" + amount);
    }
}

// Encapsulation
class Order {
    private String orderId;
    private List<Item> items;
    private double totalAmount;
    private PaymentMethod paymentMethod;
    
    public boolean checkout(PaymentMethod payment) {
        this.paymentMethod = payment;
        return payment.processPayment(totalAmount);
    }
    
    public double getTotalAmount() {
        return totalAmount;
    }
}

// Inheritance
class Product {
    String name;
    double price;
    String category;
}

class Electronics extends Product {
    int warrantyPeriod;
    String brand;
}

class Clothing extends Product {
    String size;
    String color;
}
```

---

### Example 2: Hospital Management System

```java
// Abstract class
abstract class Person {
    String name;
    int age;
    String contactNumber;
    
    abstract void getRole();
    
    void displayInfo() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}

// Inheritance
class Doctor extends Person {
    String specialization;
    List<String> availableSlots;
    
    void getRole() {
        System.out.println("Role: Doctor");
    }
    
    void prescribeMedicine(Patient patient, String medicine) {
        System.out.println("Prescribed " + medicine + " to " + patient.name);
    }
}

class Patient extends Person {
    String patientId;
    List<String> medicalHistory;
    
    void getRole() {
        System.out.println("Role: Patient");
    }
    
    void bookAppointment(Doctor doctor, String slot) {
        System.out.println("Appointment booked with Dr. " + doctor.name);
    }
}

// Interface for different departments
interface Department {
    void admitPatient(Patient patient);
    void dischargePatient(Patient patient);
}

class EmergencyDepartment implements Department {
    public void admitPatient(Patient patient) {
        System.out.println("Emergency admission for " + patient.name);
    }
    
    public void dischargePatient(Patient patient) {
        System.out.println("Emergency discharge for " + patient.name);
    }
}
```

---

## Interview Tips

### How to Explain OOP Concepts in Interview

**Structure your answer:**
1. **Define** the concept clearly
2. **Explain** why it's important
3. **Give a real-world example**
4. **Show code** if asked

**Example Answer Template:**

> "**Encapsulation** is the bundling of data and methods within a class while restricting direct access to some components. It's important because it provides **data hiding, validation, and maintainability**. 
>
> For example, in a banking system, we keep the account balance private and provide public methods like deposit() and withdraw(). This way, we can validate transactions, maintain audit logs, and prevent unauthorized access. Users can't directly set balance to any value - they must go through controlled methods."

---

### Common Interview Questions

1. **What are the main principles of OOP?**
   - Encapsulation, Inheritance, Polymorphism, Abstraction

2. **Why use private variables with public methods instead of public variables?**
   - Validation, control, flexibility, security

3. **What's the difference between interface and abstract class?**
   - Use the comparison table above

4. **Can you achieve multiple inheritance in Java?**
   - Yes, through interfaces (not with classes due to diamond problem)

5. **What is the diamond problem?**
   - Ambiguity when a class inherits from two classes with the same method

6. **Real-time example of polymorphism?**
   - Payment processing, delivery systems, notification services

7. **When to use abstract class vs interface?**
   - Abstract class: Common implementation + IS-A relationship
   - Interface: Contract definition + unrelated classes

---

## Key Takeaways

✅ **Encapsulation** = Data hiding + Controlled access  
✅ **Inheritance** = Code reusability + IS-A relationship  
✅ **Polymorphism** = One interface, multiple implementations  
✅ **Abstraction** = Hide complexity, show essentials  
✅ **Interface** = Contract for unrelated classes  
✅ **Abstract Class** = Partial abstraction with shared code  

---

## Contributing

Feel free to contribute by adding more examples or improving existing ones!

---

## License

This guide is for educational purposes. Feel free to use and share!

---

**Good luck with your interviews! 🎯**

*Remember: Understanding concepts with real-world examples is more important than memorizing definitions.*