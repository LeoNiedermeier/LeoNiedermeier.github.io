---
title: Jackson Json Subtype Mapping
tags: [java_misc]
category: java_misc
summary: " Jackson Json Subtype Mapping"
---

# Overview

If an object hierarchy should be serialized and deserialized to/from Json, type information has to be provided:
* in the Json structure in order to identify the concrete type
* to the jackson (de-)serializer, so that for instance the deserializer can instantiate the target type.

Example data structure:
~~~java
class Wrapper {
    Vehicle[] vehicles;
}

class Vehicle {
    String name;
}

Car extends Vehicle {
    String horsePower;
}

class Bicycle extends Vehicle {
    String color;
}
~~~

# JsonTypeInfo and JsonSubTypes Annotations

With the help of the `JsonTypeInfo` and `JsonSubTypes` the class hierarchy can be (de)serialized. The annotations have to be 
applied to the super-class. 

{: .danger title="Cyclic Dependencies"}
The `JsonSubTypes` with `Type` annotations result in a cyclic dependency between super-class and sub-class.


The type info is stored in a property due to `JsonTypeInfo.As.PROPERTY` and `property = "@type"`:

~~~java
@JsonTypeInfo(use = JsonTypeInfo.Id.NAME, include = JsonTypeInfo.As.PROPERTY, property = "@type")
@JsonSubTypes({ @Type(value = Car.class, name = "car"), @Type(value = Bicycle.class, name = "bicycle"), })
class Vehicle {
    ...
}
~~~

Example output:
~~~json
{
  "vehicles" : [ {
    "@type" : "vehicle",
    "name" : "myVehicle"
  }, {
    "@type" : "car",
    "name" : "myCar",
    "horsePower" : "23"
  } ]
}
~~~


The same with type info as `WRAPPER_OBJECT`:

~~~java
@JsonTypeInfo(use = JsonTypeInfo.Id.NAME, include = JsonTypeInfo.As.WRAPPER_OBJECT, property = "@type")
@JsonSubTypes({ @Type(value = Car.class, name = "car"), @Type(value = Bicycle.class, name = "bicycle"), })
class Vehicle {
    ...
}
~~~

Example output:
~~~json
{
  "vehicles" : [ {
    "my-vehicle" : {
      "name" : "myVehicle"
    }
  }, {
    "my-car" : {
      "name" : "myCar",
      "horsePower" : "99"
    }
  } ]
}
~~~


# Explicit Registering of Types

With `ObjectMapper.registerSubtypes(...)` jackson provides the possibility to explicitly register subtypes. This has to be done 
in the configuration step of the `ObjectMapper`.

In order to simplify and generalize this functionality, we can use classpath scanning.
We use the `JsonTypeName` annotation as marker for classes which should be registered as subtypes.
{: .info title="No Cyclic Dependencies"}
With this approach we do not have cyclic dependencies anymore. Furthermore, it is possible to add additional sub-classes from 
other modules / jars without the need of changing the configuration.

~~~java
@JsonTypeInfo(use = JsonTypeInfo.Id.NAME, include = JsonTypeInfo.As.PROPERTY, property = "@type")
@JsonTypeName("vehicle")
class Vehicle {
    ...
}
@JsonTypeName("car")
Car extends Vehicle {
    ...
}

@JsonTypeName("bicycle")
class Bicycle extends Vehicle {
    ...
}
~~~

## Spring Classpath Scanning

With the help of `ClassPathScanningCandidateComponentProvider` and `AnnotationTypeFilter` we can scan for all classes in 
the classpath which have a `JsonTypeName` annotation. From the resulting `BeanDefinition`s it is straight forward to construct 
a `NamedType`:

~~~java
List<Class<?>> findClasses(String basePackage) {
    ClassPathScanningCandidateComponentProvider provider = new ClassPathScanningCandidateComponentProvider(false);
    provider.addIncludeFilter(new AnnotationTypeFilter(JsonTypeName.class));
     return provider.findCandidateComponents(basePackage).stream().map(beanDefinition -> {
            try {
                return Class.forName(beanDefinition.getBeanClassName());
            } catch (ClassNotFoundException e) {
                throw new IllegalStateException("Class not found " + beanDefinition.getBeanClassName(), e);
            }
        }).collect(Collectors.toList());
    }
~~~

## Classgraph Classpath Scanning

See <https://github.com/classgraph/classgraph>

~~~java
List<Class<?>> findClasses(String basePackage) {
    try (ScanResult scanResult = new ClassGraph().enableClassInfo().enableAnnotationInfo()
                                   .ignoreClassVisibility().whitelistPackages(basePackage).scan()) {
        ClassInfoList classInfoList = scanResult.getClassesWithAnnotation(JsonTypeName.class.getName());
        return classInfoList.loadClasses();
    }
}
~~~

# References

* <https://github.com/classgraph/classgraph>