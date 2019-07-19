---
title: ContextedRuntimeException
tags: [java, java_misc]
category: java_misc
summary: "ContextedRuntimeException"
---
# Overview
Throwing a exception through through multiple layers of an application can add some more or less useful information.

**Exception enhancement**  
Whenever it is necessary to catch an exception in order to add some additional information to the exception and re-throw it,
 no new exception is created rather then the original exception is enhanced with some context information.

**Exception Wrapping**  
Catch a exception and throw a new exception with additional information and the original exception.
 This results usually in a long and 'stacked' stack trace, but most of the time, only the stack trace of
 the original exception is interesting. The other stacktraces are just noise.


**Terminology**  
A system **failure** is an event that occurs when the delivered service deviates from correct service.
 A system may fail either because it does not comply with the specification, or because the specification
 did not adequately describe its function. An **error** is that part of the system state that may cause
 a subsequent failure: a failure occurs when an error reaches the service interface and alters the service.
 A **fault** is the adjudged or hypothesized cause of an error.  
(<https://softwareengineering.stackexchange.com/questions/184412/whats-the-difference-between-fault-error-and-defect/184418>)

# Implementation

An easy way to implement exception enhancement is to use a `ContextedRuntimeException` class
 like <https://github.com/LeoNiedermeier/io.github.leoniedermeier.utils/blob/master/src/main/java/io/github/leoniedermeier/utils/excecption/ContextedRuntimeException.java>:


## Output Structure

Printing a `ContextedRuntimeException` gives a message of the kind:

{: .code title="Example output of ContextedRuntimeException"}
~~~
ErrorCodes: CODE_1, CODE_3
CID: QQAWFR
---------------------------------
Descriptions:
  CODE_1: Description CODE 1
  CODE_3: Description CODE 3
---------------------------------
Exception Context:
  ErrorCode: CODE_1
  CID: QQAWFR
  label: MyValue
  ErrorCode: CODE_3
---------------------------------
    at io.github.leoniedermeier.utils.excecption.ContextedRuntimeExceptionTest$ThrowsException.doSomething(ContextedRuntimeExceptionTest.java:25)
    at io.github.leoniedermeier.utils.excecption.ContextedRuntimeExceptionTest.lambda$1(ContextedRuntimeExceptionTest.java:44)
    ...
~~~

The output consists of the following elements:
* General information
  * Error codes
  * CID: Correlation id
* Description: optional detail description of error codes
* Exception Context: the data of the exception context
* A stacktrace
  
# Correlation ID
The correlation id (CID) is unique per `ContextedRuntimeException` instance. For instance the CID can help to trace the exception 
through remote calls and their logs. 

## Error Codes

In order to have a unique identifier for the exception type, the constructor of `ContextedRuntimeException` requires an `ErrorCode`.

{: .code .x title="ErrorCode"}
~~~java
public interface ErrorCode {
    String code();
    ...
}
~~~

Note that there can be more additional error codes added (via `ContextedRuntimeException#addErrorCode(ErrorCode)`). This can be useful in the case of exception enhancement.

In general the error codes are implemented as enums in the root package of the module. The enums have to implement `EnumErrorCode`. 
In this case the error code is the name of the enum constant.

{: .code title="Error code example"}
~~~java
enum MyErrorCodes implements EnumErrorCode {
    MISSING_FILE,
    WRONG_INPUT;
}
~~~ 

## Context Values

The context values are a list of label-value pairs. Label is a `String`, value an `Object`.
 Note that there can be multiple elements with the same label.

The context values can provide additional information in order to support problem diagnosis.


# How to use

## New ContextedRuntimeException

{: .code .x title="Example new ContextedRuntimeException"}
~~~java
try {
      ...
    } catch (Exception e) {
      throw new ContextedRuntimeException(MY_ERROR_CODE, e)
           .addContextValue("Account Number", accountNumber)
           .addContextValue("Amount Posted", amountPosted)
           .addContextValue("Previous Balance", previousBalance)
}
~~~

## With Exception Enhancement and Rethrow

{: .code .x title="Example exception enhancement"}
~~~java
private void dataBase()
{
    throw new ContextedRuntimeException(MY_ERROR_CODE)
                      .addContextValue("query", "select * from table");
}


public void service() {
    try {
        dataBase();
    }
    catch(ContextedRuntimeException exception) {
        // enhance the current exception and rethrow it
        throw exception.addContextValue("Service", "Context information of service");
    }
}
~~~

# Some Implementation Notes

## Add a Exception Context to a Custom Exception

* The custom exception has to implement the interface `ExceptionContext` or `ExtendedExceptionContext`.
* The implementation of the method `getContextEntries` has to return a list where the context `Entry`s can be stored.
* The implementation of `getMessage` should delegate to  `getFormattedExceptionMessage(...)` in order to print the context 
  values in the message.   

{: .code .x title="CustomContextedRuntimeException"}
~~~
public class CustomContextedRuntimeException extends RuntimeException
        implements ExtendedExceptionContext<ContextedRuntimeException> {

    /** Where the data is stored. */
    private final List<Entry> entries = new ArrayList<>();

    @Override
    public List<Entry> getContextEntries() {
        return entries;
    }

    /**
     * Provides the message explaining the exception, including the contextual data.
     *
     * @see java.lang.Throwable#getMessage()
     * @return the message, never {@code null}.
     */
    @Override
    public String getMessage() {
        return getFormattedExceptionMessage(super.getMessage());
    }
    ...
}
~~~ 

## Add a HttpResponseStatus to the Error Code Enum
The `@HttpResponseStatus` annotation ( 
 <https://github.com/LeoNiedermeier/io.github.leoniedermeier.utils/blob/master/src/main/java/io/github/leoniedermeier/utils/web/advice/HttpResponseStatus.java>)
 adds a http response status (`org.springframework.http.HttpStatus`) to the error code enum.

{: .code .x title="HttpRepsonseStatus"}
~~~
enum MyErrorCodes implements EnumErrorCode {
    ERROR_CODE_SIMPLE,

    @HttpResponseStatus(HttpStatus.NOT_FOUND)
    ERROR_CODE_WITH_HTTP_NOT_FOUND;
}
~~~

### The Spring Controller Advice
See <https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc>

The http response status of the error code enum of the `ContextedRuntimeException` can be evaluated in a controller advice, 
for instance
<https://github.com/LeoNiedermeier/io.github.leoniedermeier.utils/blob/master/src/main/java/io/github/leoniedermeier/utils/web/advice/RestExceptionHandler.java>

{: .code .x title="Controller Advice"}
~~~
public ResponseEntity<ErrorInformation> handle(ContextedRuntimeException exception) {

    ErrorInformation errorInformation = ...;

    errorInformation.setStatus(exception.findLastErrorCode().filter(ec -> ec instanceOf Enum<?>)
        // (enum) field with same id as code() returns
        .map(ec -> findField(ec.getClass(), ec.code()))
        .map(field -> field.getAnnotation(HttpResponseStatus.class))//
        .map(HttpResponseStatus::value).orElse(HttpStatus.INTERNAL_SERVER_ERROR));
    ...
}
~~~