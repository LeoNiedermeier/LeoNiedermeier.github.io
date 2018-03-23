---
title: Overload Method Resolution
tags: [java]
category: OCAP
summary: Overload Method Resolution
---

# Übersicht

*Overloading*    
> If two methods of a class ... have the same name but signatures that are not override-equivalent, then the method name is said to be overloaded.    
(<https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.4.9>)

{: .info title=""override-equivalent"}
> Two methods or constructors, M and N, have the same signature if they have the same name, the same type parameters (if any) (§8.4.4),
> and, after adapting the formal parameter types of N to the the type parameters of M, the same formal parameter types.
>
> The signature of a method m1 is a subsignature of the signature of a method m2 if either:
>
>* m2 has the same signature as m1, or    
>* the signature of m1 is the same as the erasure (§4.6) of the signature of m2.    
>
>Two method signatures m1 and m2 are override-equivalent iff either m1 is a subsignature of m2 or m2 is a subsignature of m1.    
>(<https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.4.2>)


# Referenzen

* <https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html>
* <https://docs.oracle.com/javase/specs/jls/se8/html/jls-15.html#jls-15.12>
