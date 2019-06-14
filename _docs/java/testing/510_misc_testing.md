---
title: Misc Testing
tags: [testing]
category: testing
summary: "Misc Testing"
---

# Creating an Instance of an Abstract Class

With <http://objenesis.org/>:

{: .info title="No Constructor Called"}
"Objenesis aims to overcome these restrictions by bypassing the constructor on object instantiation."    
(from <http://objenesis.org/index.html>)    

Note: Spring (re)packs objensis in spring-core in the `org.springframework.objenesis` package.

~~~java
// simple but not recommended
ObjenesisHelper.newInstance(MyClass.class);
~~~


~~~java
Objenesis objenesis = new ObjenesisStd();
ObjectInstantiator<MyClass> instantiator = objenesis.getInstantiatorOf(MyClass.class);
MyClass newInstance = instantiator.newInstance();
...
~~~