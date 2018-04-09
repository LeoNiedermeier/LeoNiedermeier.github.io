---
title: Enum
tags: [java]
category: OCAP
summary: Enum
---

# Allgemein

Format (in dieser Reihenfolge):

* enum const list
* enum body

# Sonstiges

* It is a compile-time error if the same keyword appears more than once as a modifier for an enum declaration.
* It is a compile-time error if a constructor declaration in an enum declaration is public or protected (default und private ok)
* It is a compile-time error if a constructor declaration in an enum declaration contains a superclass constructor invocation statement    
 (also kein super() Aufruf im Konstruktor möglich, aber thsi(...)) 
* It is a compile-time error to attempt to explicitly instantiate an enum type     
(Konstruktor kann nicht explizit aufgerufen werden (nie new MyEnum() möglich)) 
* Kann in switch-case verwendet werden
* Implementiert `Comparable`, das auf `Enum.ordinal` geht


# Beispiele

Enum mit Class-Body (<https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.9>)

~~~java
enum Operation {
    PLUS {
        double eval(double x, double y) { return x + y; }
    },
    MINUS {
        double eval(double x, double y) { return x - y; }
    },
    // Each constant supports an arithmetic operation
    abstract double eval(double x, double y);
}
~~~

# Referenzen

* <https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.9>
