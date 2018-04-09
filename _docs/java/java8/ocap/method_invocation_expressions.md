---
title: Methods Invocation Expression
tags: [java]
category: OCAP
summary: Methods Invocation Expression
---


# Einfache Auflösung bei Aufruf über Methodennamen

+  Determine Class or Interface to Search:    
   Sucht die Klasse / Interface in der die Methode mit gegebenen Namen vorhanden ist. (Spezialfall z.B. innere Klassen)

+ Determine Method Signature:    
  * Overload resolution **without** permitting boxing or unboxing conversion,
  * Overload resolution while allowing boxing and unboxing,
  * Overloading to be combined with variable arity methods, boxing, and unboxing.
  

# Beispiele

~~~java
void foo(int x) {}
void foo(Object x)  {}

// ruft foo(Object x): 
foo(new Integer(1)) // da passende Signatur ohne(!) Autoboxing
foo(1L) // Autoboxing nach Long, dann passt foo(Object)

// ruft foo(int x) auf
foo('c') // char 'x' widening nach int
~~~

# Referenzen

* <https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html#jls-15.12>
