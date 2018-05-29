---
title: Access Modifiers
tags: [java]
category: ocap
summary: Access Modifiers
---

private -> default -> protected -> public


## Beispiele

Default Modifier schränkt Sichtbarkeit der Methode foo mit ein, ist die Subklasse aber public, dann wird diese Einschränkung aufgehoben. PublicSubclass#foo ist public. 
~~~java
class Base {
	public void foo() {}
}

public class PublicSubclass extends Base {
}
~~~
