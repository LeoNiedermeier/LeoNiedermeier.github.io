---
title: How to Determine Where a Java Class is Loaded From
tags: [eclipse]
category: eclipse
summary: "Notes about Eclipse"
---

This can be done by changing the detail formatter of the class to
~~~java
getClass().getProtectionDomain().getCodeSource().getLocation(); 
~~~

In the variables view of the debug perspective open the context menu:

![set detail formatter](class_loading_location/setDetailFormatter.png "set detail formatter")

Set the detail formatter

![](class_loading_location/detailFormatter.png)


{: .danger title="Tip"}
Do not forget to reset the detail formatter afterwards

# References
* <http://henryranch.net/tutorials/how-to-determine-where-a-java-class-is-being-loaded-from/>
