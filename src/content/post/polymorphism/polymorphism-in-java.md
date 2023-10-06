---
title: "Polymorphism in Java"
description: "Understanding in detail do's and dont's of Java Polymorphism"
publishDate: "4 May 2023"
coverImage:
  src: "./Java-image.jpeg"
  alt: "Java Tech Image"
slug: polymorphism-in-java
tags: ["Java", "Polymorphism"]
---

## Tricky Polymorphism

- Lets first understand what is allowed :
	- Child cObj = new Parent();    => compile time error
	- Parent pObj = new Child();   => compiles, called upcasting

- In upcasting, only methods defined in Parent is allowed. Calling child method on pObj which is not defined in Parent class, will give compile time error. 

	Check out this example :
	
	```java
	class Parent {
		public void print() {
			System.out.println("Parent");
		}
	}
	
	class Child extends Parent {
		public void print() {
			System.out.println("Child");
		}

		public void test() {
			System.out.println(
				"Child method. Not defined in Parent.");
		}
	}
	
	class MainClass {
		public static void main (String[] args) {
			Parent pObj = new Child();
			pObj.test();  //compile time error
		}
	}
	```
	But how does it work?

### Understanding compile-time and runtime type
- There are two types of 'Type' of object in Java : Compile-time and runtime. In above example, compile-time type of pObj is Parent class; while runtime type of pObj is Child class.

- Sequence of events in Overriding :
	- At compile time, JVM first checks the method presence in compile-time type of object.
	- Then binds it with compile-time type method. Binding happens with most specific method, in case of overloaded method. 
	- Then at runtime, overriding that exact method signature by runtime type method (of Child class), happens.
- In simple terms, in overriding, Child class method with exact signature (runtime type) is invoked. 

- Now test yourself with this example :

	```java 
	class Parent {
	    public void print(Object i) {
	        System.out.println("Parent int : " + i);
	    }
	}

	class Child extends Parent {
		@Override
	    public void print(Object i) {
	        System.out.println("Child Object : " + i);
	    }
	    
	    public void print(int i) {
	        System.out.println("Child int: " + i);
	    }
	}

	class MainClass {
		public static void main (String[] args) {
		    Parent pObj = new Child();
		    pObj.print(1); 
			Child cObj = new Child();
		    cObj.print(1);
		}
	}
	```
	Let's pause and think.
	<br>
	<br>
	<br>
	<br>
	<br>
	<br>

	Answer is : 
	```java
	Child Object : 1       
	//Coz exact signature method is overridden at runtime
	Child int: 1      
	//Overloading, neither hiding nor overriding Parent method
	```
- From this we learn one more thing.<br>In a Child class, we can overload the methods inherited from Parent. Such overloaded methods neither hide nor override the Parent methods — they are new methods, unique to the Child.

Ok. All good till now. <br>But how does this changes in case of static methods?

### Incase of static methods
- Sequence of events for overriding static methods :
	- At compile time, first method presence is checked in compile-time type, 
	- then binding with compile-time type method (most specific in case of overloaded method) 
	- but NO runtime overriding. Hence its not called overriding, its called method hiding. 
- In simple terms, incase of static method, Parent class method (compile time type) is invoked.
- One more thing to note.<br><a name="note">An instance method of Child cannot override a static method of Parent. And a static method of Child cannot hide an instance method of Parent.</a> Both will give compile time error. Try thinking why.


### FAQs
1. _Can we Override static methods by differing method input params?_<br>
   **NO.** We can declare such methods but its just another method, not an overriding behavior.
2. _Can we Override methods that differ only by static keywords?_<br>
   **NO.** This will give compile time error. Refer [here](#note).
3. _Can we Override static methods with the same signature in the subclass?_<br>
   **NO.** We can declare such methods but its not an overriding behavior, its hiding. No runtime polymorphism.
4. _Can we Overload static methods?_ <br>
   **YES.** By differing method input param.
5. _Can we Overload methods that differ only by static keywords?_<br>
   **NO.** This will give compile time error.

<br>
Hope this cleared some doubts on how Polymorphism in Java works. Please hit me up on socials if you want to add something to this article. <br><br>Thanks for reading and
ABC - Always be Coding!