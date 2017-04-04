---
title: Angular Routing
tags: [angular]
category: angular
summary: "Angular Routing"
---

# Overview

WORK IN PROGRESS

??? Open Point: URL and Bookmarking:
The URL changes due to rooting. How to handle bookmarking?

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

# Fetch data With Resolver

For details see also <https://angular.io/docs/ts/latest/guide/router.html#!#resolve-guard>

In the rout definition we have a `resolve` property defined. The property is an object literal. The properties of this object describe the data. The value of a property is the resolver class.

~~~typescript
// route definition
{ path: 'myPath', component: MyComponent, resolve: { myData: MyDataResolver } },
~~~

The `MyDataResolver` has to be defined a a `provider` od a `NgModule`:

~~~typescript
@NgModule({
  ...
  providers: [MyDataResolver]
})
~~~

The `MyResolver` class implements the `Resolve` interface.

~~~typescript
export class MyResolver implements Resolve<MyData> {
  
  constructor() {
  }

  resolve(routeSnapshot: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<MyData> {
    // here we have access to parameters via e.g. routeSnapshot.params['myParam'];
    // return Observable<MyData> object.
    return ...;
  }
}

~~~

# Subscribe to Resolved Data in the Component

For details see also <https://angular.io/docs/ts/latest/guide/router.html#!#resolve-guard>

The `ActivatedRoute` object is injected into the constructor of the component. The data provided by the resolver can be retrieved in the component by subscribing to the `ActivatedRoute.property`. 

~~~typescript
constructor(private route: ActivatedRoute, ....)
...
route.data.subscribe((data: { myData: MyData }) => {
      // We do change the array, therefore copy it in other not change the original array
      let inputData: MyData = data.myData;
      ...
    });
~~~

{: .info title="Nested Path"}
The resolved data is provided by a parameter with the name 'data'. The resolved objects are nested in this object. Therefore we have to access the resolved data with the path prefix 'data.'. In the example the resolbed object is stored in the 'myData' property. Therefore we have to access is `data.myData`.

## Miscellaneous

### Access to URL Parameters

The simples way to accessed url parameters is via the `ActivatedRouteSnapshot` or `ActivatedRoute` objects. 

~~~typescript
route: ActivatedRoute = ....
route.snapshot.params['myParam']
~~~

# References

* <https://angular.io/docs/ts/latest/guide/router.html>
