---
title: Custom Validators
tags: [spring, spring_boot]
category: spring
summary: "Custom Validators in Spring"
---

# Overview

Validation of method parameters of spring beans (`@Controller`, `@Service` etc) 

## Prerequisites

* Spring Boot
* Provided by `spring-boot-starter-web`:
  * `org.hibernate.validator:hibernate-validator`
  * `javax.validation:validation-api`

### Note
Spring Boot provides the required infrastructure beans `org.springframework.validation.beanvalidation.MethodValidationPostProcessor` and `org.springframework.validation.beanvalidation.LocalValidatorFactoryBean`. 

## The Validation Annotation

The annotation for the validation, for instance`@MyValidationConstraint`, must have the attributes `message`, `groups` and `payload`. For details see javadoc of `javax.validation.Constraint`.

~~~ java
@Target({ ElementType.METHOD, ElementType.PARAMETER })
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy = MyValidationConstraintValidator.class)
// https://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/?v=6.0#validator-customconstraints-simple
public @interface MyValidationConstraint {
    
    // message interpolation: see https://docs.jboss.org/hibernate/stable/validator/reference/en-US/html_single/?v=6.0#chapter-message-interpolation
    String message() default "The value ${validatedValue} is not valid.";

    Class<?>[] groups() default {};

    Class<? extends Payload>[] payload() default {};
}
~~~

## The Validator Implementation

The validator `MyValidationConstraintValidator` can be implemented as a spring bean:

~~~ java
@Component
public class MyValidationConstraintValidator implements ConstraintValidator<MyValidationConstraint, String> {

    private MyService myService;

    public StockSymbolConstraintValidator(final MyService myService) {
        this.myService = myService;
    }

    @Override
    public boolean isValid(final String value, final ConstraintValidatorContext context) {
        // can use myService to test value
        
        return true  or false;
    }

}
~~~


## Usage in Service Method

The bean which method parameter should be validatet must be annotated with `@org.springframework.validation.annotation.Validated`. The method parameter to check must be annotated with `MyValidationConstraint`.

~~~ java
@Service
@Validated
public class SomeService {
  
  public ReturnObject serviceMethod(@MyValidationConstraint String symbol) {
  ...
  }
}
~~~

# References

* <https://docs.spring.io/spring/docs/5.0.x/spring-framework-reference/core.html#validation-beanvalidation>
