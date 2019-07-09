---
title: Mockito
tags: [testing]
category: testing
summary: "Mockito"
---

# Inject Mock Objects as Method Parameters


* Useful when multiple test methods need mock objects.
* Saves some trivial mock object creation code.


Traditional mock creation:

~~~java
class MyTest {

    void test() {
        MyObject myObjectMock = Mockito.mock(MyObject.class);
        ...
    }
}
~~~


With mock object injection in Junit 5:

~~~java
@ExtendWith(MockitoExtension.class)
class MyTest {

    void test(@Mock MyObject myObjectMock) {
        ...
    }
}
~~~

* `@ExtendWith(MockitoExtension.class)`: Use the mockito Junit 5 extension
* `@Mock`: Annotation for test method parameters. Injects a mock object.  


# Argument Matching with Hamcrest Matchers

Use the static methods of `MockitoHamcrest` for creating mockito `ArgumentMatcher` from a hamcrest `Matcher`


~~~java
class MyService {
   public String someMethod(String string) { ... }
}

@Test
void hamcrestArgumentMatcher(@Mock MyService myService) {

    Mockito.when(myService.someMethod(MockitoHamcrest.argThat(Matchers.startsWith("ABC")))).thenReturn("123");
 
    String result = myService.someMethod("ABCDE");

    assertEquals("123", result);
    Mockito.verify(myService).someMethod(MockitoHamcrest.argThat(Matchers.startsWith("ABC")));
}
~~~

# Mock with `org.mockito.stubbing.Answer<T>`

The `org.mockito.stubbing.Answer<T>` can be implemented via:

* inner class
* lambda expression
* method reference
* `AdditionalAnswers`  

{: .code title="MyService"}
~~~java 
class MyService {
    public String someMethod(String string) { ... }
}
~~~

{: .code title="Inner class"}
~~~java 
@Test
void mockWithAnswer(@Mock MyService mock) {
    Mockito.when(mock.someMethod(ArgumentMatchers.anyString())).then(new Answer<String>() {
        @Override
        public String answer(InvocationOnMock invocation) {
            Object[] args = invocation.getArguments();
            return "Mock:" + args[0];
        }
    });
    assertEquals("Mock:foo", mock.someMethod("foo"));
}
~~~

{: .code title="Lambda"}
~~~java

@Test
void mockWithLambdaAnswer(@Mock MyService mock) {
    Mockito.when(mock.someMethod("foo"))
      .then(invocation -> {
        Object[] args = invocation.getArguments();
        return "Mock:" + args[0];
    });
    assertEquals("Mock:foo", mock.someMethod("foo"));
}
~~~

{: .code title="Method Reference"}
~~~java
@Test
void mockWithMethodReferenceAnswer(@Mock MyService mock) {
    Mockito.when(mock.someMethod("foo")).then(this::answerMethod);
    assertEquals("Mock:foo", mock.someMethod("foo"));
}

private String answerMethod(InvocationOnMock invocation) {
    Object[] args = invocation.getArguments();
    return "Mock:" + args[0];
}
~~~


`AdditionalAnswers` provides factory method for `Answer`s. 
In particular the an `Answer` can be created from functional interfaces which mimic the mocked method 
with respect to its parameters. No `InvocationOnMock` needed. Nice to combine with method references.

{: .code title="AdditionalAnswers"}
~~~java
@Test
void mockWithAdditionalAnswers(@Mock MyService mock) {
    Mockito.when(mock.someMethod(ArgumentMatchers.anyString()))
    // or as Method reference... AdditionalAnswers.answer("Mock:"::concat)
    .then(AdditionalAnswers.answer((String s) -> "Mock:" + s));
    
    assertEquals("Mock:foo", mock.someMethod("foo"));
}
~~~
# References

* <https://site.mockito.org/>