---
title: Overload, Override, Hide
tags: [java]
category: OCAP
summary: Overload Method Resolution
---

# Übersicht

{: .info title="Overloading"}
> If two methods of a class (whether both declared in the same class, or both inherited by a class, or one declared and one inherited) have the same name but signatures that are not override-equivalent, then the method name is said to be overloaded.    
> (from <https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.4.9>)


{: .info title="override-equivalent"}
> Two methods or constructors, M and N, have the same signature if they have the same name, the same type parameters (if any) (§8.4.4),
> and, after adapting the formal parameter types of N to the the type parameters of M, the same formal parameter types.
>
> The signature of a method m1 is a **subsignature** of the signature of a method m2 if either:
>
>* m2 has the same signature as m1, or    
>* the signature of m1 is the same as the erasure (§4.6) of the signature of m2.    
>
> Two method signatures m1 and m2 are **override-equivalent** iff either m1 is a subsignature of m2 or m2 is a subsignature of m1.    
>(from <https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.4.2>)

* It is a compile-time error to declare two methods with override-equivalent signatures in a class.

* An abstract class can override an abstract method by providing another abstract method declaration.

+ A class does not inherit static methods from its superinterfaces.


# Beispiele

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

# Referenzen

* <https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html>
* <https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html#jls-15.12>
