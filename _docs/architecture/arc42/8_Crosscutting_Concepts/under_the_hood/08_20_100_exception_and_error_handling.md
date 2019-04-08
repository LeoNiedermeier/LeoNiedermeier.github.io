---
title: Exception und Error Handling
tags: [arg42]
category: arc42
summary: "Exception und Error Handling"
---


# Übersicht

Werden Exceptions geworfen, so sollten für die Fehlerauswertung möglichst viele (ausreichende) Informationen zur Verfügung stehen. 
Es muss nachvollziebar sein, wo und unter welchen Umständen (Kontext) ein Fehler auftrat. Mit einem einfachen `throw new RuntimeException("Eine 
Meldung")` ist das in der Regel nicht zu erreichen.

Ferner ist eine Einheitlichkeit des Fehlerhandlings über die Anwendung anzutreben. Damit wird es auch einfacher, Fehler automatisiert 
aufzuspühren und auszuwerten. 

Siehe auch  "Maximize Human Participation" from "Patterns for Fault Tolerant Software" (<https://www.oreilly.com/library/view/patterns-for-fault/9780470319796/>)

# Checked  / Unchecked Exceptions

In der Regel werden unchecked Exceptions verwendet. Checkt Exceptions werden nur in bestimmten Ausnahmefällen verwendet. Diese 
können sein:

* Der Aufrufer kann beim Auftreten der Exceptionen den Fehler behandeln
* Der Aufrufer muss (wird dazu gezwungen) die Exception zu behandeln.  

Vergleiche auch ""Effective Java 3rd Edition":
* "Item 70: Use checked exceptions for recoverable conditions and runtime exceptions for programming errors"
* "Item 71 - Avoid unnecessary use of checked exceptions"

# Custom Exceptions und Exception Hierarchie

Spezielle Exceptions bzw. eine Hierarchie von Exceptions werden nur dann erzeugt, wenn diese Exceptions eine spezielle Behandlung 
in einen `catch` Block haben sollten / könnten.

# Logging
In der Regel werden beim Erzeugen oder catch / rethrow von Exceptions *keine* Logausgaben gemacht. Logausgaben werden gemacht, 
wenn eine Exception gefangen und nicht weitergeworfen wird. Dies macht meiste der übergeordnete Exceptionhandler.


# ContextedRuntimeException
In der Regel wird eine `ContextedRuntimeException` mit individuellem Errorkode verwendet. Dies hat folgende Vorteile:

* Exception Enhancement (siehe unten)
* Einheitliche und formatierte Fehlermeldung
* Kontext Elemente möglich
* Über einen eindeutigen ErrorCode kann man die Fehler und die Fehlerstelle identifizieren (auch ohne Stacktrace)
* Mittels einer Excpetion Instanz spezifischen Korrelation-ID kann der Aufrufpfad nachverfolgt werden. (ggf. auch über Remote 
  Aufrufe hinweg)    

## Errorkodes

Errorkodes sind über die Anwendung eindeutig. Sie setzten sich aus einem modul-spezifischem Präfix und einem modul-eindeutigem 
Kode zusammen, z.B. `MYMODULE_ABCX1`. Die Errorkodes werden in einem modulspezifischem `enum` im root-Package des Moduls definiert.


## Beispiele

### ContextedRuntimeException mit Kontextwerten
Neu erzeugte und geworfene `ContextedRuntimeException` sollten ausreichend Kontextinformationen für Fehlerauswertung bereitstellen. Mit 
`addContextValue` kann man beliebig viele Kontextwerte ergänzen.

~~~java
throw new ContextedRuntimeException(MY_ERROR_CODE)
    .addContextValue("myLabel", myValue)
    .addContextValue("otherLabel", otherValue);
~~~

### Exception Enhancement
Mittels `catch` und `throw` kann man zu vorhandenen `ContextedRuntimeException` zusätzliche Kontextwerte ergänzen. Dies 
kann an solchen Stellen nütlich sein, an denen für die Fehlerauswertung relevante Informationen zur Verfügung stehen. 

~~~java
try {
    ...
catch (ContextedRuntimeException e) {
    e.addContextValue("key", myValue);
    throw e;
}
~~~

### Exception Wrapping
Exception Wrapping sollte weitgehend vermieden weden. Speziell bei mehrfachem Exception Wrapping wird der Stacktrace sehr 
unübersichtlich, teilweise werden wichtige Informationen abgeschnitten. Die ursprüngliche Exception ist nicht mehr auf den ersten 
Blick identifizierbar.

Bei `ContextedRuntimeException` ist Exception Enhancement zu bevorzugen.

# Error Handling

## Lokal
Lokales Error Handling findet statt, wenn an der entsprechenden Stelle die Exception korrekt behandelt werden kann.

## Zentral

Beim Übergang an Schnittstellen bei denen es nicht vorgesehen ist, dass Exception geworfen werden, muß ein Exception Handling 
stattfinden.

Beispiele

* Rest Schnittstelle
* ...

### Spring Controller Advice  
Bei einem Rest-Aufruf wird im Falle eines Fehlers folgende Header zurückgeliefert:

* `X-HEADER-ERROR-CODES`: Liste der Errorkodes
* `X-HEADER_CID`: Eine Korrelation ID
  
Der Spring `RestControllerAdvice`  `RestExceptionHandler` verarbeitet `ContextedRuntimeException` und setzt die entsprechenden 
Header.


# Referenzen

* <https://www.oreilly.com/library/view/patterns-for-fault/9780470319796/>
* <http://tutorials.jenkov.com/java-exception-handling/checked-or-unchecked-exceptions.html>
* <http://tutorials.jenkov.com/java-exception-handling/checked-or-unchecked-exceptions.html>