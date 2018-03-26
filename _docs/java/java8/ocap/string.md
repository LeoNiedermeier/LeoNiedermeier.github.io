---
title: Strings etc
tags: [java]
category: OCAP
summary: Strings etc.
---

~~~java
public class Test {
  public static void main(String[] args) {
    MyToString mts = new MyToString();
    String s = "X" + mts;
}

class MyToString {
  public String toString() {
    throw new NullPointerException("From toString");
  }
}
~~~

Stacktrace: schön zu sehen, dass aus `"X" + j` ein irgendwas mit`StringBuilder.append` compiliert wird:

~~~java
Exception in thread "main" java.lang.NullPointerException: From toString
	at test.MyToString.toString(Test.java:15)
	at java.lang.String.valueOf(String.java:2994)
	at java.lang.StringBuilder.append(StringBuilder.java:131)
	at test.Test.main(Test.java:6)
~~~
