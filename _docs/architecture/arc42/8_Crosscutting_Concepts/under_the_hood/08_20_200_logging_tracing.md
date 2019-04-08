---
title: Logging und Tracing
tags: [arg42]
category: arc42
summary: "Logging und Tracing"
---

# Übersicht

# Log4j 2
<https://logging.apache.org/log4j/2.x/manual/api.html>

Für den Nutzer sind insbesondere folgende Features von Vorteil:

* Parameter Substitution
* Lambda Support

# Einbindung von anderen Logging Frameworks

3rd Party Libraries benutzen andere Logger, zum Beispiel:

* Apache Commons Logging (https://commons.apache.org/proper/commons-logging/)
* Java Util Logger (https://docs.oracle.com/javase/8/docs/api/java/util/logging/package-summary.html)

Die Ausgaben dieser Logger sollten ebenfalls über Log4j 2 laufen.

## Apache Commons Logging

* Spring benutzt "Apache Commons Logging"  
* Module "spring-jcl": 
    * "A minimal incarnation of Apache Commons Logging's {@code LogFactory} API, 
    providing just the common `Log` lookup methods. ..."
    * "It also serves as a common bridge for third-party libraries using the Commons Logging API ..."

## Java Util Logging
Java Util Logging wird über den Log4j JDK Logging Adapter angebunden: <https://logging.apache.org/log4j/2.x/log4j-jul/index.html>


# Verwendung
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
