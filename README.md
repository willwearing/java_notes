# java_notes

- If we want to remove elements from an `ArrayList` while traversing through one, we can easily run into an error if we aren’t careful. When an element is removed from an `ArrayList`, all the items that appear after the removed element will have their index value shift by negative one
    - Remove using a `while` loop:

    ```java
    int i = 0; // initialize counter
     
    while (i < lst.size()) {
      // if value is odd, remove value
      if (lst.get(i) % 2 != 0){
        lst.remove(i);
      } else {
        // if value is even, increment counter
        i++;
      }
    }
    ```

    - We do not increase our loop control variable whenever we remove an element. This ensured that we would not skip an element when all of the other elements shifted to the left.
    - In a `for` loop, the control variable will always increase by 1 at the end of each loop. Therefore, we must therefore manually decrease the control variable:

    ```java
    for (int i = 0; i < lst.size(); i++) {
      if (lst.get(i) == "value to remove"){
        // remove value from ArrayList
        lst.remove(lst.get(i));
        // Decrease loop control variable by 1
        i--;    
      }
    }
    ```

    ### String Methods

    - We don’t have to import anything to use the String class because it’s part of the `java.lang` package
    - The most common `String` methods are:

    ```java
    length()
    concat()
    equals()
    indexOf()
    charAt()
    substring()
    toUpperCase() / toLowerCase()
    ```

    - Here are some basic examples in action:

    ```java
    public class HelloWorld {
    	public static void main(String[] args) {
        String str = "Hello, World!";
        // Examples
        System.out.println(str.length());
        System.out.println(str.substring(4));
        System.out.println(str.toUpperCase());
      }  
    }
    // 13
    // o, World!
    // HELLO, WORLD!

    ```

    - `equals()` checks if a string is equal to another string. We can also compare `String` values lexicographically (think dictionary order) using the `.compareTo()` method. When we call the `.compareTo()` method, each character is in the `String` is converted to Unicode; then the Unicode character from each `String` is compared. The method returns an `int` that represents the difference between the two strings:

    ```java
    String flavor1 = "Mango";
    String flavor2 = "Peach"; 
    System.out.println(flavor1.compareTo(flavor2));
    // -3
    ```

    - "Mango" comes before "Peach", so we get a negative number (we specifically get -3 because the Unicode values of "M" and "P" differ by 3). If we did `flavor2.compareTo(flavor1)`, we would get 3, signifying that "Peach" is greater than "Mango".
    - When we use `.compareTo()`, we must pay attention to the return value:
        - If the method returns `0`, the two `String`s are equal.
        - If the value is less than `0`, then the `String` object is lexicographically less than the `String` object argument.
        - If the value is greater than `0`, then the `String` object is lexicographically greater than the `String` object argument.
    - `.concat()` doesn't change the value of the `String` (strings are immutable). To change the value of the `String`, make sure to create a new variable and set it to that

    ### Access and Scope

    - We can define the access of many different parts of our code including instance variables, methods, constructors, and even a class itself
    - If we choose to declare these as `public` this means that any part of our code can interact with them - even if that code is in a different class!
    - When a Class’ instance variable or method is marked as `private`, that means that you can only access those structures from elsewhere inside that same class
    - Here's an easy example:

    ```java
    public class DogSchool{
     
      public void makeADog(){
        Dog cujo = new Dog("Cujo", 7);
        System.out.println(cujo.age);
        cujo.speak();
      }
    }
    ```

    - `makeADog` is trying to directly access `Dog`‘s .`age` variable. It’s also trying to use the `.speak()` method. If those are marked as private in the `Dog` class, the `DogSchool` class won’t be able to do that. Other methods within the `Dog` class would be able to use `.age` or `.speak()` (for example, we could use `cujo.age` within the `Dog` class), but other classes won’t have access.
    - Why make code `private`? Restricting our code is actually useful from a design perspective. This is one of the core ideas behind encapsulation. By making our instance variables (and some methods) `private`, we encapsulate our code into nice little bundles of logic. By limiting access by using the `private` keyword, we are able to segment, or encapsulate, our code into individual units.
    - To get a proper understanding, consider the following 2 java files that make up a basic bank account:

    ```java
    public class Bank{
      private CheckingAccount accountOne;
      private CheckingAccount accountTwo;

      public Bank(){
        accountOne = new CheckingAccount("Zeus", 100);
        accountTwo = new CheckingAccount("Hades", 200);
      }

      public static void main(String[] args){
        Bank bankOfGods = new Bank();
        System.out.println(bankOfGods.accountOne.name);
        bankOfGods.accountOne.addFunds(5);
        bankOfGods.accountOne.getInfo();
      }
    }

    // Bank.java:12: error: name has private access in CheckingAccount
    //  System.out.println(bankOfGods.accountOne.name);                                        ^
    // Bank.java:13: error: addFunds(int) has private access in CheckingAccount
    //  bankOfGods.accountOne.addFunds(5);               ^
    // Bank.java:14: error: getInfo() has private access in CheckingAccount
    //  bankOfGods.accountOne.getInfo();                    ^
    // 3 errors
    ```

    ```java
    public class CheckingAccount{
      private String name;
      private int balance;

      public CheckingAccount(String inputName, int inputBalance){
        name = inputName;
        balance = inputBalance;
      }

      private void addFunds(int fundsToAdd){
        balance += fundsToAdd;
      }

      private void getInfo(){
        System.out.println("This checking account belongs to " + name +". It has " + balance + " dollars in it.");
      }

      public static void main(String[] args){
        CheckingAccount myAccount = new CheckingAccount("Will", 2000);
        System.out.println(myAccount.balance);
        myAccount.addFunds(5);
        System.out.println(myAccount.balance);
        myAccount.getInfo();
      }
    }

    // 2000
    // 2005
    // This checking account belongs to Will. It has 2005 dollars in it.
    ```

    - **Accessor and Mutator Methods**
    - Instance variables in Java are non-static variables which are defined in a class outside any method, constructor or a block. Each instantiated object of the class has a separate copy or instance of that variable (instances and objects are the same thing)
    - When writing classes, we often make all of our instance variables private. However, we still might want some other classes to have access to them, we just don’t want those classes to know the exact variable name. To give other classes access to a private instance variable, we would write an accessor method (sometimes also known as a “getter” method):

    ```java
    public class Dog{
      private String name;
     
      //Other methods and constructors
     
      public String getName() {
        return name;
      }
    }
    ```

    - Even though the instance variable name is private, other classes could call the public method `getName()` which returns the value of that instance variable. Accessor methods will always be public, and will have a return type that matches the type of the instance variable they’re accessing
    - Similarly, `private` instance variables often have mutator methods (sometimes known as “setters”). These methods allow other classes to reset the value stored in `private` instance variables

    ```java
    public class Dog{
      private String name;
     
      //Other methods and constructors
     
      public void setName(String newName) {
        name = newName;
      }
     
      public static void main(String[] args){
        Dog myDog = new Dog("Cujo");
        myDog.setName("Lassie");
      }
    }
    ```

    - Mutator methods, or “setters”, often are void methods — they don’t return anything, they just reset the value of an existing variable. Similarly, they often have one parameter that is the same type as the variable they’re trying to change
    - **this - keyword**
    - The `this` keyword is a reference to the current object:

    ```java
    public class Dog{
      public String name;
     
      public Dog(String inputName){
        name = inputName;
      }
     
      public void speakNewName(String name){
        System.out.println("Hello, my new name is" + this.name);
      }
     
      public static void main(String[] args){
        Dog a = new Dog("Fido");
        Dog b = new Dog("Odie");
     
        a.speakNewName("Winston");
        // "Fido", the instance variable of Dog a is printed. "Winston" is ignored
     
        b.speakNewName("Darla");
        // "Odie", the instance variable of Dog b is printed. "Darla" is ignored.
      }
    }
    ```

    - We used [`this.name`](http://this.name/) in our `speakNewName()` method. This caused the method to print out the value stored in the instance variable name of whatever Dog Object called `speakNewName()`. (Note that in this somewhat contrived example, the local variable name used as a parameter gets completely ignored).
    - The most common use of the this keyword is to eliminate the confusion between class attributes and parameters with the same name (because a class attribute is shadowed by a method or constructor parameter)
    - For a simple of example of `this` in use:

    ```java
    public class SavingsAccount{

      public String owner;
      public int balanceDollar;
      public double balanceEuro;

      public SavingsAccount(String owner, int balanceDollar){
        // Complete the constructor
        this.owner = owner;
        this.balanceDollar = balanceDollar;
        this.balanceEuro = balanceDollar * 0.85;

      }

      public void addMoney(int balanceDollar){
        // Complete this method
        System.out.println("Adding " + balanceDollar + " dollars to the account.");
        this.balanceDollar = this.balanceDollar + balanceDollar;
        System.out.println("The new balance is " + this.balanceDollar + " dollars.");
      }

      public static void main(String[] args){
        SavingsAccount zeusSavingsAccount = new SavingsAccount("Zeus", 1000);

        // Make a call to addMoney() to test your method
        zeusSavingsAccount.addMoney(2000);
      }

    }
    // Adding 2000 dollars to the account.
    // The new balance is 3000 dollars.
    ```

    - You can also use `this` with methods:

    ```java
    public class Computer{
      public int brightness;
      public int volume;
     
      public void setBrightness(int inputBrightness){
        this.brightness = inputBrightness;
      }
     
      public void setVolume(int inputVolume){
        this.volume = inputvolume;
      }
     
      public void resetSettings(){
        this.setBrightness(0);
        this.setVolume(0);
      }
    }

    ```

    - This method calls other methods from the class. But it needs an object to call those methods! Rather than create a new object, we use `this` as the object. What this means is that the object that calls `resetSettings()` will be used to call `setBrightness(0)` and `setVolume(0)`
    - `this` can be used as a value for a parameter
    - Let’s say a method exists that takes a `Computer` as a parameter (that method’s signature might be something like `public void pairWithOtherComputer(Computer other)`. If you’re writing another method of the `Computer`, and want to call the `pairWithOtherComputer()` method, you could use this as the parameter. That call might look something like `this.pairWithOtherComputer(this)`. You’re using the current object to call the method and are passing that object as that method’s parameter

    ```java
    public void pairWithOtherComputer(Computer other){
      // Code for method that uses the parameter other
    }
     
    public void setUpConnection(){
      // We use "this" to call the method and also pass "this" to the method so it can be used in that method
      this.pairWithOtherComputer(this);
    }
    ```

    - In review:
        - The `public` and `private` keywords are used to define what parts of code have access to other classes, methods, constructors, and instance variables
        - Encapsulation is a technique used to keep implementation details hidden from other classes. Its aim is to create small bundles of logic
        - The `this` keyword can be used to designate the difference between instance variables and local variables
        - Local variables can only be used within the scope that they were defined in
        - The `this` keyword can be used to call methods when writing classes
-
