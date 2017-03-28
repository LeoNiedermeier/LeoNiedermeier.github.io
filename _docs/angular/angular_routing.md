---
title: Angular Routing
tags: [angular]
category: angular
summary: "Angular Routing"
---

# Overview

WORK IN PROGRESS

## routerLink Directive

A static link target uses the `routerLink` without binding:

~~~html
 <a  routerLink="my-target" routerLinkActive="active">Static Link</a>
~~~


A dynamically calculated target uses the `[routerLink]`. 
Add

~~~
<tr class="" *ngFor="let myElement of someArray; let i = index">
    ...
    <a [routerLink]="['/my-target', { 'parameter1': myElement.property1}]">Dynamic Link with matrix variables</button>
    ...
</tr>    
~~~

results in `/myTarget;parameter1=propertyValue1`.

# References

* <https://angular.io/docs/ts/latest/guide/router.html>
