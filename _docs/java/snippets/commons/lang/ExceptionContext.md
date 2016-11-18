---
title: ExceptionContext
tags: [java, apache_commons]
category: apache_commons
summary: "Some example usages of the org.apache.commons.lang3.exception.ExceptionContext class."
---

# Overview
Throwing a exception through through multiple layers of an application can add some more or less useful information.

**Exception enhancement**  
Whenever it is necessary to catch an exception in order to add some additional information to the exception and re-throw it, no new exception is created rather then the original exception is enhanced with some context information.

**Exception Wrapping**
Catch a exception and throw a new exception with additional information and the original exception. This results usually in a long and 'stacked' stack trace, but most of the time, only the stack trace of the original exception is interesting. The other stacktraces are just noise.

An easy way to implement exception enhancement is to use the commons lang `ExceptionContext` classes:

* `org.apache.commons.lang3.exception.ContextedRuntimeException` for unchecked exceptions and
* `org.apache.commons.lang3.exception.ContextedException` for checked exceptions.


# How to use

```java
private void dataBase()
{
	throw new ContextedRuntimeException("Exception thrown from DataBase")
	              .addContextValue("query", "select * from table");
}


public void service() {
	try
	{
		dataBase();
	}
	catch(ContextedRuntimeException exception)
	{
		System.out.println(exception.getMessage());
		/* prints
		Exception thrown from DataBase
        Exception Context:
	    [1:query=select * from table]
        ---------------------------------
		*/
		throw exception.addContextValue("Service", "Context information of service");
	}
}
public void outer() {
    try
	{
		service();
	}
	catch(ContextedRuntimeException exception)
	{
		/* prints
		Exception thrown from DataBase
        Exception Context:
	    [1:query=select * from table]
	    [2:Service=Context information of service]
	    ---------------------------------
		*/
		System.out.println(exception.getMessage());
		}
	}
}
```

{: .wpanel-info title="Method chaining"}
>The `addContextValue`method of the `ContextedException` and `ContextedRuntimeException` returns the current instance. Therefore it is easy to add additional context values, especially when catching and re-throwing a exception.
>
```java
try {
      ...
    } catch (Exception e) {
      throw new ContextedRuntimeException("Error posting account transaction", e)
           .addContextValue("Account Number", accountNumber)
           .addContextValue("Amount Posted", amountPosted)
           .addContextValue("Previous Balance", previousBalance)
}
```


# References
* <http://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/exception/ExceptionContext.html>
* <http://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/exception/ContextedException.html>
* <http://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/exception/ContextedRuntimeException.html>
* <http://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/exception/DefaultExceptionContext.html>

Some links from <http://jenkov.com/>
* [Exception Enrichment in Java](http://tutorials.jenkov.com/java-exception-handling/exception-enrichment.html "Exception Enrichment in Java")
* [Java Exception Handling](http://jenkov.com/books/java-exception-handling/index.html, "Java Exception Handling")
