---
title: Hamcrest
tags: [testing]
category: testing
summary: "Hamcrest"
---

# Overview

How to write custom hamcrest matchers

* <http://hamcrest.org/JavaHamcrest/tutorial>
* <https://www.vogella.com/tutorials/Hamcrest/article.html>

# Custom Matchers

Referenced classes are part of the project `io.github.leoniedermeier.utils` (<https://github.com/LeoNiedermeier/io.github.leoniedermeier.utils/tree/master/src/main/java/io/github/leoniedermeier/utils/test/hamcrest>).

## The TransformingMatcher Class
The `TransformingMatcher` applies a `Function<T,R>` to the initial value and calls a 
`Matcher<R>` for the transformed value (note that the `TransformingMatcher` is a `Matcher<T>` ).

~~~java
  assertThat("abcd", new TransformingMatcher<String, Integer>(String::length, "myText", Matchers.greaterThan(2)));
~~~

### PropertyAccess Class
The method `PropertyAccess.property(Function<T, R> transformer, String description)` returns an interface with the single method 
`Matcher<T> is(Matcher<? super R> matcher)`. These two methods can be combined to replace a constructor call of `TransformingMatcher`.   

~~~java
assertThat("abcd", PropertyAccess.property(String::length, "length of string").is(Matchers.greaterThan(2)));

// or with static imports
assertThat("abcd", property(String::length, "length of string").is(greaterThan(2)));
~~~

{: .code .x title="Example with bean properties"}
~~~java

// Modell classes
class Person {
    private final List<Phone> phones = new ArrayList<>();
    public void addPhone(Phone phone) {
        phones.add(phone);
    }
    public List<Phone> getPhones() {
        return phones;
    }
}

class Phone {
    private String number;

    public Phone(String number) {
        this.number = number;
    }

    public String getNumber() {
        return number;
    }
}

// Factory method for reuseable Matchers
static Matcher<Person> allPhoneNumbersStartWith(String string) {
    return property(Person::getPhones, " person phones ")
                .is(everyItem(property(Phone::getNumber, " number ").is(Matchers.startsWith(string))));
}

// Test setup
Person person = new Person("Name");
person.addPhone(new Phone("0049-1234"));
person.addPhone(new Phone("0049-ABCD"));

// Test method
assertThat(person, allPhoneNumbersStartWith("0049"));
~~~


## ExceptionMatchers Class
The class `io.github.leoniedermeier.utils.test.hamcrest.ExceptionMatchers` provides a method `throwA`. This method returns a 
matcher for a `ExceptionMatchers.Executable`. An `Executable` is a functional 
interface with a single none arguments method which can throw an `Exception`.

~~~java
assertThat(() -> myMethod(arg1, arg2), throwsA(SQLException.class)));
~~~


~~~java
assertThat(() -> myMethod(arg1, arg2), throwsA(SQLException.class)));
~~~

With the `with` method of the `ExceptionMatcher` is is possible to define additional matchers. This matchers apply to the thrown exception.

~~~java
assertThat(() -> myMethod(arg1, arg2), throwsA(SQLException.class)).with(matcherForException) ));
~~~

Together with the property matcher feature, one can write a matcher like:

~~~java
assertThat(() -> myMethod(arg1, arg2), 
      throwsA(SQLException.class)).
          with(property(SQLException::getSQLState, " sql-state ").is(equalTo("Expected-SQL-STATE"))));
~~~

It checks whether the method throws a `SQLException` and the `getSQLState` method of `SQLException` returns "Expected-SQL-STATE".

# References

* <http://hamcrest.org/>
* <https://www.baeldung.com/tag/hamcrest/>
* <https://www.baeldung.com/java-junit-hamcrest-guide>  