

Wäre eine Zusammenfassung für die Umsetzung

# Formatierung
# Spring Dependency Injaction
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