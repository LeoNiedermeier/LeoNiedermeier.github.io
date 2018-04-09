---
title: Access Modifiers
tags: [java]
category: OCAP
summary: Access Modifiers
---

private -> default -> protected -> public


## Beispiele

Default Modifier schränkt Sichtbarkeit der Methode foo mit ein, ist die Subklasse aber public, dann wird diese Einschränkung aufgehoben.
~~~java
class Base {
	public void foo() {}
}

public class AccessModifiersSubclass extends Base {
}
~~~
