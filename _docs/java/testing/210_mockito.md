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


With mock object injection:

~~~java
@ExtendWith(MockitoExtension.class)
class MyTest {

    void test(@Mock MyObject myObjectMock) {
        ...
    }
}
~~~



# References

* <https://site.mockito.org/>