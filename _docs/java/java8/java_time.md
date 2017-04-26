---
title: Java 8 Time
tags: [java,java8]
category: java8
summary: Java 8 time"
---


# Instant und Period

Aus Javadoc zu InstantKlasse

> An instantaneous point on the time-line.  
(<https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html>)

Damit ist Instant inetwa sowas wie `java.util.Date`

Period

> A date-based amount of time in the ISO-8601 calendar system, such as '2 years, 3 months and 4 days'.
This class models a quantity or amount of time in terms of years, months and days. 
See Duration for the time-based equivalent to this class.  
(<https://docs.oracle.com/javase/8/docs/api/java/time/Period.html>)

Duration

> A time-based amount of time, such as '34.5 seconds'. This class models a quantity or amount of time 
in terms of seconds and nanoseconds.  
(<https://docs.oracle.com/javase/8/docs/api/java/time/Duration.html>)

{: .info title="Hinweis"}
Ein Monat (Period) kann eine Duration zwischen 28*86400s = 2419200s und 31*86400=2678400s haben 
(Schalt-Stunden nicht berücksichtigt).

## Beispiele
### Instant
~~~java
Instant.now();  
Instant.ofEpochSecond(120000)  
~~~
 
 
Umwandlung von `java.util.Date` in `Instant` und umgekehrt geht mit folgenden Methoden:

~~~java
java.util.Date.toInstant()  
java.util.Date.from(Instant)  
~~~
 
### Duration

~~~java
Duration duration = Duration.between(Instant.ofEpochSecond(0L), Instant.ofEpochSecond(120000));  
System.out.println(duration.toHours()); // 33  

Duration duration2 = duration.multipliedBy(2).minusDays(10);  
System.out.println(duration2.toHours()); // -173  
System.out.println(duration2.isNegative()); // true  
~~~

In der Klasse Duration gibt es Methoden zum addieren, subtrahieren, multiplizieren von Durations 
(es wird immer eine neues Duration Objekt erzeugt - Duration ist immutable)


# LocalDate und LocalTime

Aus Javadoc zu LocalDate

> A date without a time-zone in the ISO-8601 calendar system, such as 2007-12-03.  
(<https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html>)

Anders als `java.util.Date` hat `LocalDate`

* keine Timezone
* nur Datum, d.h. Jahr, Monat und Tag

Damit eigenet sich `LocalDate` deutlich besser für Datumsangaben, z.B. Geburtsdatum

LocalTime

> A time without time-zone in the ISO-8601 calendar system, such as 10:15:30.  
(<https://docs.oracle.com/javase/8/docs/api/java/time/LocalTime.html>)

 

LocalDateTime

> A date-time without a time-zone in the ISO-8601 calendar system, such as 2007-12-03T10:15:30.  
(<https://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html>)

## Beispiele

 
### LocalDate

~~~java
LocalDate today = LocalDate.now(); // 2014-07-01  
System.out.println(today);  
  
LocalDate date_1 = LocalDate.of(2010, 1, 1); // 2010-01-01  
System.out.println(date_1);  

LocalDate date_2 = LocalDate.ofYearDay(2010, 100); // 2010-04-10  
System.out.println(date_2);  

LocalDate date_3 = LocalDate.ofYearDay(2012, 100); // 2012-04-09 (Schaltjahr)  
System.out.println(date_3);  

LocalDate date_4 = LocalDate.of(2010, Month.FEBRUARY, 10);  
System.out.println(date_4);  

// java.time.DateTimeException: Invalid date 'FEBRUARY 30'  
// LocalDate.of(2010, Month.FEBRUARY, 30);  
~~~

### LocalTime

~~~java
LocalTime time_1 = LocalTime.now(); // 11:04:35.113  

LocalTime time_2 = LocalTime.of(22, 30); // 22:30  

LocalTime time_3 = time_2.plusHours(8); // 06:30  

System.out.println(time_3.isAfter(time_2)); // false  
System.out.println(time_3.isBefore(time_2)); // true  
~~~
 
### Period

~~~java
LocalDate date_1 = LocalDate.of(2010, Month.JANUARY, 10); // 2010-03-10  
LocalDate date_2 = LocalDate.of(2011, Month.APRIL, 23); // 2011-04-23  
  
Period period = date_1.until(date_2);  
System.out.println(period); // P1Y3M13D = 1 Year 3 Month 13 Days  

long periodInDays = date_1.until(date_2, ChronoUnit.DAYS);  
System.out.println(periodInDays); // 468  
~~~  

Dasselbe in einem Schaltjahr: 

~~~java
LocalDate date_1 = LocalDate.of(2012, Month.JANUARY, 10); // 2012-03-10  
LocalDate date_2 = LocalDate.of(2013, Month.APRIL, 23); // 2013-04-23  

Period period = date_1.until(date_2);  
System.out.println(period); // P1Y3M13D  = 1 Year 3 Month 13 Days  

long periodInDays = date_1.until(date_2, ChronoUnit.DAYS);  
System.out.println(periodInDays); // 469  
~~~ 

{: .info title="Hinweis"}
In diesem Beispiel sieht man sehr schön den Unterschied Period und die Zeitdifferenz in Tagen. 
Geht die Differenz über den 29. Februar eines Schaltjahres, so bleibt die die Period gleich, die Differenz in 
Tagen wird aber eins größer (um den 29. Februar).

# Formatierung

Javadoc DateTimeFormatter

>    Formatter for printing and parsing date-time objects.
...
A formatter created from a pattern can be used as many times as necessary, it is immutable and is thread-safe.  
From <https://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html>

 
## Beispiele

Man beachte die Unterschiede bei ZonedDateTime und LocalDateTime!

~~~java

DateTimeFormatter.ISO_DATE_TIME.format(LocalDateTime.now()); // 2014-07-22T15:18:47.328  
DateTimeFormatter.ISO_DATE_TIME.format(ZonedDateTime.now()); // 2014-07-22T15:18:47.328+02:00[Europe/Berlin]  
DateTimeFormatter.ISO_ZONED_DATE_TIME.format(ZonedDateTime.now()); // 2014-07-22T15:18:47.328+02:00[Europe/Berlin]  

DateTimeFormatter.ISO_LOCAL_DATE.format(LocalDate.now()); // 2014-07-22  
DateTimeFormatter.ISO_DATE.format(ZonedDateTime.now()); // 2014-07-22+02:00  

DateTimeFormatter formatter = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.FULL);  
formatter.format(ZonedDateTime.now()); // Dienstag, 22. Juli 2014 15:24 Uhr MESZ  

// LocalDateTime nur SHORT, MEDIUM, für Long, FULL kann keine Zeitzone bestimmt werden ;  
DateTimeFormatter formatter = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.MEDIUM);  
formatter.format(LocalDateTime.now()); // 22.07.2014 15:24:56  

DateTimeFormatter.ofPattern("dd.MM.yyyy", Locale.GERMANY).format(LocalDate.now()); // 22.07.2014  
DateTimeFormatter.ofPattern("dd.MM.yyyy", Locale.GERMANY).format(ZonedDateTime.now()); // 2014-07-22+02:00  

LocalDate date = LocalDate.parse("22.07.2014", DateTimeFormatter.ofPattern("dd.MM.yyyy")); // 2014-07-22  
~~~
 
 
# Temporal Adjusters

> Adjusters are a key tool for modifying temporal objects.  
(<https://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAdjuster.html>)

Man kann damit neue temporale Objekte erzeugen, die in Beziehung zum Ausgangsobjet stehen, z.B.

* den nächsten Monatag
* den ersten des folgenden Monats
* den nächsten Zahltag (Cusom Adjuster, <http://docs.oracle.com/javase/tutorial/displayCode.html?code=http://docs.oracle.com/javase/tutorial/datetime/iso/examples/NextPayday.java>)
* ...

 
 ~~~java
LocalDate date_1 = LocalDate.of(2010, Month.FEBRUARY, 10); // 2010-02-10  
LocalDate date_2 = date_1.with(TemporalAdjusters.nextOrSame(DayOfWeek.MONDAY)); // 2010-02-15  
LocalDate date_3 = date_1.with(TemporalAdjusters.firstDayOfNextMonth()); // 2010-03-01  
~~~
 
# Referenzen

* https://docs.oracle.com/javase/8/docs/api/java/time/Instant.html
* <hhttps://docs.oracle.com/javase/8/docs/api/java/time/Period.html>
* <https://docs.oracle.com/javase/8/docs/api/java/time/Duration.html>
* <http://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html>
* <http://docs.oracle.com/javase/8/docs/api/java/time/LocalTime.html>
* <http://docs.oracle.com/javase/8/docs/api/java/time/LocalDateTime.html>
* <http://docs.oracle.com/javase/8/docs/api/java/time/format/DateTimeFormatter.html>
* <http://docs.oracle.com/javase/8/docs/api/java/time/temporal/TemporalAdjusters.html>
* <http://docs.oracle.com/javase/tutorial/displayCode.html?code=http://docs.oracle.com/javase/tutorial/datetime/iso/examples/NextPayday.java> 
 
