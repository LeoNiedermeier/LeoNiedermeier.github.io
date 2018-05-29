---
title: Methods - Overload, Override, Hide
tags: [java]
category: ocap
summary: Overload Method Resolution
---

# Allgemeine Begriffe

{: .info title="override-equivalent"}
> Two methods or constructors, M and N, have the same signature if they have the same name, the same type parameters (if any),
> and, after adapting the formal parameter types of N to the the type parameters of M, the same formal parameter types.
>
> The signature of a method m1 is a **subsignature** of the signature of a method m2 if either:
>
>* m2 has the same signature as m1, or    
>* the signature of m1 is the same as the erasure (§4.6) of the signature of m2.    
>
> Two method signatures m1 and m2 are **override-equivalent** iff either m1 is a subsignature of m2 or m2 is a subsignature of m1.    
>(from <https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.4.2>)

* It is a compile-time error to declare two methods with override-equivalent signatures in a class


# Overloading

{: .info title="Overloading"}
> If two methods of a class (whether both declared in the same class, or both inherited by a class, or one declared and one inherited) have the same name but signatures that are **not override-equivalent**, then the method name is said to be overloaded.    
> (from <https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.4.9>)


# Overriding

{: .info title="Overiding Methods from Super Class"}
> An instance method mC declared in or inherited by class C, overrides from C another method mA declared in class A, 
> iff all of the following are true:
>
> * A is a superclass of C.
> * C does not inherit mA.
> * The signature of mC is a subsignature (§8.4.2) of the signature of mA.
> 
> ... und access modifier müssen passen

* It is a compile-time error if an instance method overrides a static method.
Beispiel:

~~~java
class CA {
  public static void doSomething() {}
}

class CB extends CA {
  public void doSomething() {} // compiler error: instance method cannot override the static method 
}
~~~

{: .info title="Overiding Methods from Implemented Interface" }
> An instance method mC declared in or inherited by class C, overrides from C another method mI declared in an interface I, 
> iff all of the following are true:
>
> * I is a superinterface of C.
> * mI is an abstract or default method.
> * The signature of mC is a subsignature (§8.4.2) of the signature of mI.


* Falls eine Methode, die in mehreren Interfaces dekalerirt wird, implementiert wird, dann muss jede einzelne override passen (insbesondere falls checked exceptions auftreten). 

**Beispiel**

override-equivalent Methoden in Interface (static) und implementierender Klasse:

~~~java
interface I1 {
  static void doSomething() {}
}

class C1 implements I1 {
  public void doSomething() {}
}
~~~

`c1.doSomething()` ruft die Methode der Instanz von `C1` auf. Die statische Methode des Interfaces `I1` kann nur mit `I1.doSomething()` angesprochen werden. Im Prinzip sind das zwei verschiedene Methoden.

Hinweis: statische Methoden eines Interfaces können nur über das Interface angesprochen werden: `MyInterface.myStaticMethod(...)`. Das ist anderas als bei statischen methoden eine Klasse. Diese können auch über eine Instanz der Klasse angesprochen werden `myInstance..myStaticMethod(...)`.

**Beispiel**

override-equivalent Methoden in Interface (default) und implementierender Klasse:

~~~java
interface I1 {
  default void doSomething() {}
}

class C1 implements I1 {
  public void doSomething() {}
}
~~~
Methode der Klasse überschreibt die default-Methode des Interfaces.

# Hiding

{: .info title="Hiding (by Class Methods)"}
> If a class C declares or inherits a static method m, then m is said to **hide** any method m', where the signature of m is 
> a subsignature of the signature of m', in the superclasses and superinterfaces of C that would otherwise be accessible to code in C.
>
> It is a compile-time error if a static method hides an instance method.

"Hide" geht also nur mit statischen Methoden.

**Beispiel**

~~~java
class C1 {
	static public void doSomething() {
	}
}

class C2 extends C1 {
	static public void doSomething() {
	}
}
~~~


# Requirements in Overriding and Hiding

{: .info title="Covariant Return Types" }
> If a method declaration d1 with return type R1 overrides or hides the declaration of another method d2 with return type R2, 
> then d1 must be return-type-substitutable for d2, or a compile-time error occurs.
>
> (This rule allows for covariant return types - refining the return type of a method when overriding it).


* *return-type-substitutable* siehe: <https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.4.5>
* Bei Klassen sind das Sub-Klassen (Interfaces)
* Bei primitives: keine Kovarianz (d.h überschreibende Methoden müssen den gleichen primitiven Return Type haben) 


{: .info title="Exceptions"}
> A method that overrides or hides another method, including methods that implement abstract methods defined in interfaces,
> may not be declared to throw more checked exceptions than the overridden or hidden method.

# Beispiel

| Super Klasse |Sub Klasse | Beschreibung |
|:--------------------|:-----------------|:--------|
| void foo()          | void foo()        | ok      |
| static void foo()   | void foo()        |  instance method cannot override the static method    |
| void foo()          | static void foo()  | static method cannot hide the instance method |
| static void foo()         | static void foo() | ok (hiding)      |
| static final void foo()   | static void foo() | Eclipse compiler: Cannot override the final method  |


# Referenzen

* <https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html>
* <https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html#jls-15.12>
