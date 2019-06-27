

Wäre eine Zusammenfassung für die Umsetzung

# Formatierung
# Spring Dependency Injection


Siehe <https://docs.spring.io/spring/docs/current/spring-framework-reference/core.html#beans-dependencies>:
> #### Constructor-based or setter-based DI?
> Since you can mix constructor-based and setter-based DI, it is a good rule of thumb 
to use constructors for mandatory dependencies and setter methods or configuration methods for optional dependencies.     
...    
The Spring team generally advocates constructor injection, as it lets you implement 
application components as immutable objects and ensures that required dependencies are not null.    
...    
Setter injection should primarily only be used for optional dependencies that can be assigned reasonable default values within the class. 
...

<div class="danger x" title="Direct Field Injection" markdown="1">

**Direkt Field in Injection ist in jedem Falle zu vermeiden.**

~~~java
public class MyClass {

    @Autowired
    private MyDependency myDependency;
}
~~~

</div>
# Tests
# Exceptions

# Logging

-> Oder siehe ....

<https://logging.apache.org/log4j/2.x/manual/api.html>

`Logger` wird als (private) statische Variable mit Namen "LOGGER" abgelegt. Der Parameter in `getLogger(...)` ist die umgebende Klasse:
~~~java
private static final Logger LOGGER = LogManager.getLogger(MyClass.class);
~~~


## Eclipse Template für Logger
~~~java
${:import(org.apache.logging.log4j.Logger, org.apache.logging.log4j.LogManager)}

private static final Logger LOGGER = LogManager.getLogger(${enclosing_type}.class);
~~~  

## Parameter Substitution

Bei dynamischen Log Ausgaben wird **keine** String-Concatination gemacht. Vielmehr wird Parameter Substitution ggf. mit Lambda 
Ausdrücken gemacht:

Statt
~~~java
if (logger.isDebugEnabled()) {
    logger.debug("Logging in user " + user.getName() + " with birthday " + user.getBirthdayCalendar());
}
~~~

wird folgendes verwendet:
~~~java
logger.debug("Logging in user {} with birthday {}", user.getName(), user.getBirthdayCalendar());
~~~

In diesem Fall ist auch keine Abfrage des Log-Levels benötigt `if (logger.isDebugEnabled())`.