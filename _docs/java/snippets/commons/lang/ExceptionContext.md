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

## With Exception Enhancement

~~~java
private void dataBase()
{
    throw new ContextedRuntimeException("Exception thrown from DataBase")
                      .addContextValue("query", "select * from table");
}


public void service() {
    try{
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
        // enhance the current exception and rethrow it
        throw exception.addContextValue("Service", "Context information of service");
    }
}
public void outer() {
    try{
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
~~~

<div class="info" title="Method chaining" markdown="1">
The `addContextValue`method of the `ContextedException` and `ContextedRuntimeException` returns the current instance. Therefore it is easy to add additional context values, especially when catching and re-throwing a exception.
~~~java
try {
      ...
    } catch (Exception e) {
      throw new ContextedRuntimeException("Error posting account transaction", e)
           .addContextValue("Account Number", accountNumber)
           .addContextValue("Amount Posted", amountPosted)
           .addContextValue("Previous Balance", previousBalance)
}
~~~
</div>

## Working with Error Codes

In some environments an error code can be helpful in order to provide a way to identify the exception without providing to much details to the outside.

Define an interface like:

~~~java
interface ErrorCode {
    String ERROR_CODE = "ERROR_CODE";
    String code();
}
~~~

There can be multiple implementations of this interface. For instance one can use a `enun`:

~~~java
enum MyErrorCodes implements ErrorCode {
    MY_EC_1,
    MY_EC_2;

    public String code() {
        return name();
    }
}
~~~

The error codes can be added to the `ContextedRuntimeException` like any other contextes value:

~~~java
throw new ContextedRuntimeException("Some Error")//
    .addContextValue(ErrorCode.ERROR_CODE, MyErrorCodes.MY_EC_1)
    .addContextValue(ErrorCode.ERROR_CODE, MyErrorCodes.MY_EC_2);
    
/* prints
    Exception Context:
    [1:ERROR_CODE=MY_EC_1]
    [2:ERROR_CODE=MY_EC_2]
*/
~~~

Note that there can be more than one error code added. This can be useful in the case of exception enhancement.


# References
* <http://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/exception/ExceptionContext.html>
* <http://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/exception/ContextedException.html>
* <http://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/exception/ContextedRuntimeException.html>
* <http://commons.apache.org/proper/commons-lang/apidocs/org/apache/commons/lang3/exception/DefaultExceptionContext.html>

Some links from <http://jenkov.com/>

* [Exception Enrichment in Java](http://tutorials.jenkov.com/java-exception-handling/exception-enrichment.html)
