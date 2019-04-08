---
title: JUnit Testing
tags: [arg42]
category: arc42
summary: "JUnit Testing"
---

# Übersicht

# Verwendete Bibliotheken

* Junit 5: <https://junit.org/junit5/>
* Mockito: <https://junit.org/junit5/>

# Strukturierung

Die Zuordnung zwischen Testkode und zu testendem Kode sollte möglichst einfach nachvollziehbar und stringent sein. Ferner sollten 
Testklassen bzw. Testmethoden auch allgemeinen best Practices wie z.B Größe der Klasse / Methode genügen.

Simple Strukturierung nach "Testklasse pro zu testender Klasse mit diversen Testmethoden" stößt besonders im Hinblick auf Größe 
und Komplexität oft an Grenzen. 

## Testcase per Feature / Methode / Fixture / Klasse
Siehe auch:

* <http://xunitpatterns.com/Testcase%20Class%20per%20Feature.html>
* <http://xunitpatterns.com/Testcase%20Class%20per%20Fixture.html>
* <http://xunitpatterns.com/Testcase%20Class%20per%20Class.html>

### Wenige Features

Hat eine Klasse wenige Features zu testen mit entsprechend wenig Testkode, so kann man diese in einer übergeordneten Testklasse zusammenführen. 
In dieser Testklasse werden die Tests für einzelne Features mit Hilfe von Junit 5 nestetd Testklassen gruppiert.

Namenskonvention
 
* Name der Testklasse = \<Name der zu testenden Klasse\>  + "Test"
* Name der nested Testklasse: \<Name Feature / Methode\>
* Name der Testmethoden: sollten beschreiben, was getestet wird


Beispiel:
~~~java
class MyClassTest {
  @Nested
  class MyMethod {
    // Testmethoden für die Methode myMethod der Klasse MyClass
  }
 // Weitere nested Testklassen 
}
~~~

### Viele verschiedene Features

Namenskonvention: 

* Name der Testklasse = \<Name der zu testenden Klasse\> +"_" + \<Name Feature / Methode\> + "_Test"
* Name der Testmethoden: sollten beschreiben, was getestet wird

Diese Namenskonvention orientiert sich an der üblichen Namensgebung und bringt mit dem Zusatz "\_\<Name Feature\>\_" ein einfaches 
Unterscheidungskriterium. Trennung per "_" ist wichtig, um den Namen der Klasse eindeutig vom Namen des Features zu trennen.

Beispiel: MyClass_MyMethod_Test



## Testmethoden

Innerhalb einer Testmethode gibt folgende Abschnitte 

* Description: Beschreibt den Test falls notwendig.
* Setup: Testspezifischer Setup
* Exerecise: Durchführen des Tests - aufruf der zu testenden Methode
* Verify: Überprüfen der Ergebnisse. Hier finden die assert-Aufrufe statt

(siehe auch <http://xunitpatterns.com/XUnitBasics.html>)

Diese bieten eine wiederkehrende Strukturierung der Testmethoden.


Beispiel:
~~~java
// Description
...
// Setup
...
// Exercise
...
// Verify
...
~~~


# Referenzen

* <https://junit.org/junit5/>
* <https://site.mockito.org/>
* <http://xunitpatterns.com/Testcase%20Class%20per%20Feature.html>