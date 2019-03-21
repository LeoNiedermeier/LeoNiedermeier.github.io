---
title: Jaxb  SOAP WS Polymorphism
tags: [misc]
category: misc
summary: "Jaxb  SOAP WS Polymorphism"
---

# Overview

# SOAP Webservice with Schema Generation
## The `xs:extension` element

~~~xml
<xs:complexType name="vehicle" abstract="true">
  <xs:sequence>
    <xs:element name="name" type="xs:string" />
  </xs:sequence>
</xs:complexType>
<xs:complexType name="car">
  <xs:complexContent>
    <xs:extension base="tns:vehicle">
      <xs:sequence>
        <xs:element name="horsepower" type="xs:string" />
      </xs:sequence>
    </xs:extension>
  </xs:complexContent>
</xs:complexType>
<xs:complexType name="bicycle">
  <xs:complexContent>
    <xs:extension base="tns:vehicle">
      <xs:sequence>
        <xs:element name="color" type="xs:string" />
      </xs:sequence>
    </xs:extension>
  </xs:complexContent>
</xs:complexType>
~~~

The JAXB compiler generates classes like:
~~~java
@XmlType(name = "vehicle"...)
@XmlSeeAlso({Bicycle.class,Car.class})
public class Vehicle {
...
}

@XmlType(name = "car"...)
public class Car {
...
}
~~~

The `@XmlSeeAlso`: "Instructs JAXB to also bind other classes when binding this class." (from javadoc)

{: .danger title="Cyclic Dependencies"}
The `XmlSeeAlso` annotations results in a cyclic dependency between super-class and sub-class.

# Plain JAXB with Spring

Start with Java classes and add JAXB annotaions:

~~~java
@XmlRootElement(name = "vehicles")
class Vehicle { ... }

@XmlRootElement(name = "car")
class Car extends Vehicle { ... }

@XmlRootElement(name = "bicycle")
class Bicycle extends Vehicle { ... }
~~~

In this case without an annotation `XmlSeeAlso`, the `JAXBContext` has to be initialized with the relevant classes:
~~~java
JAXBContext jaxbContext = JAXBContext.newInstance(Vehicle.class, Bicycle.class, Car.class);
~~~

With the help of `org.springframework.oxm.jaxb.Jaxb2Marshaller` (which uses a `org.springframework.oxm.jaxb.ClassPathJaxb2TypeScanner`), 
it is possible to scan multiple packages (with their subpackages) for JAXB relevant classes and setup a `JAXBContext`:
~~~java
Jaxb2Marshaller marshaller = new Jaxb2Marshaller();
marshaller.setPackagesToScan(<package names>);
marshaller.afterPropertiesSet();
~~~

## Configuration Variants for Elements

### Element Names
~~~java
class Wrapper {
    @XmlElementWrapper(name = "vehicles")
    @XmlElementRef
    public Vehicle[] vehicles;
}
~~~

Result:
~~~xml
<vehicles>
    <car>
        <name>myCar</name>
        <horsePower>99</horsePower>
    </car>
    <bicycle>
        <name>myBicycle</name>
        <color>red</color>
    </bicycle>
</vehicles>
~~~

### With `xsi:type`

Without the `@XmlElementRef` the resulting xml looks like:
~~~xml
<vehicles>
    <vehicles xsi:type="car">
        <name>myCar</name>
        <horsePower>99</horsePower>
    </vehicles>
    <vehicles xsi:type="bicycle">
        <name>myBicycle</name>
        <color>red</color>
    </vehicles>
</vehicles>
~~~

# References