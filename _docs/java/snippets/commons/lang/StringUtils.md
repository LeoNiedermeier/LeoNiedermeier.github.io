---
title: StringUtils
tags: [java, apache_commons]
category: apache_commons
summary: "Some example usages of the org.apache.commons.lang3.StringUtils class."
---
# Overview

{: .info title="Null arguments"}
The methods of `org.apache.commons.lang3.StringUtils` handle null arguments in a proper way. There is no need for extra null checks.

# Some Methods

## Test for empty

**blank**: CharSequence is whitespace, empty ("") or null.  
`isBlank("111/222/333")`  
`isNoneBlank("111/222/333")`  

**empty**: CharSequence is empty ("") or null   
`isEmpty("111/222/333")`  
`isNotEmpty("111/222/333")`


## Get substrings

Gets the substring before / after the first / last occurrence of a separator. The separator is not returned.


`substringAfter("111/222/333", "/")` result: "222/333"

`substringAfterLast("111/222/333", "/")` result: "333"


`substringBefore("111/222/333", "/")` result: "111"

`substringBeforeLast("111/222/333", "/")` result: "111/222"


## Remove substring

Removes a substring at the beginning of a string    
`removeStart("111/222/333", "111")` result: "/222/333"   

Remove substring at the end of a string    
`removeEnd("111/222/333", "333")` result: "111/222/"

## Appending strings
Appends the suffix to the end of the string if the string does not already end in the suffix.  
`appendIfMissing("www.examle", ".com")` result: "www.examle.com"

Prepends the prefix to the start of the string if the string does not already start the prefixes.  
`prependIfMissing("examle.com", "www.")` result: "www.examle.com"

## Joining string
Joining elements to a single string with the `join(...)` method.  
`join(new String[]{"1","2","3"},"-")` result: "1-2-3"

## Some more methods
* `indexOf`  
* `startsWith`  
* `endsWith`  

## Escaping and unescaping strings with StringEscapeUtils

The class `org.apache.commons.lang3.StringEscapeUtils` provides methods to escape and unescape strings:

* CSV
* EcmaScript (JavaScript)
* HTML
* Java
* Json
* XML

# References
* <http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/StringUtils.html>
* <http://commons.apache.org/proper/commons-lang/javadocs/api-release/org/apache/commons/lang3/StringEscapeUtils.html>
