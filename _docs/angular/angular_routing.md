---
title: Angular Routing
tags: [angular]
category: angular
summary: "Angular Routing"
---

# Overview

Somen notes on Angular routing. The refererence documentation can be found at <https://angular.io/docs/ts/latest/guide/router.html>

## routerLink Directive

A static link target uses the `routerLink` without binding:

~~~html
 <a  routerLink="my-target" routerLinkActive="active">Static Link</a>
~~~


A dynamically calculated target uses the `[routerLink]`. The value of the attribute is a json array which can consist of strings and expressions.

~~~ html
<tr class="" *ngFor="let myElement of someArray; let i = index">
    ...
    <!-- Dynamic Link with matrix variables results in /myTarget;parameter1=propertyValue1 -->
    <a [routerLink]="['/my-target', { 'parameter1': myElement.property1}]">...</button>
    ...
    <!-- Link with multiple path segments. Static and dynamic path segments. -->
    <a [routerLink]="['/my-target', myElement.pathSegment]">Dynamic Link with matrix variables</button>
    ...
</tr>    
~~~

# Fetch data With Resolver

For details see also <https://angular.io/docs/ts/latest/guide/router.html#!#resolve-guard>

In the route definition we can define a `resolve` property. The property is an object literal. The properties of this object describe the data. The value of a property is the resolver class.

~~~ typescript
// route definition
{ path: 'myPath', 
  component: MyComponent, 
  resolve: { myData: MyDataResolver } },
~~~

The `MyDataResolver` has to be defined as a `provider` of a `NgModule`:

~~~ typescript
@NgModule({
  ...
  providers: [MyDataResolver]
})
~~~

The `MyDataResolver` class implements the `Resolve` interface.

~~~ typescript
export class MyDataResolver implements Resolve<MyData> {
  
  constructor() {
  }

  resolve(routeSnapshot: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<MyData> {
    // here we have access to parameters via e.g. routeSnapshot.params['myParam'];
    // return Observable<MyData> object.
    return ...;
  }
}

~~~

# Fetch the Resolved Data in Component

One can access the data directly from the route's snapshot data, see <https://angular.io/guide/router#snapshot-the-no-observable-alternative>

{: .info title="No Reuse"}
Remember: you only get the initial value of the parameter map with this technique. Stick with the observable paramMap approach if there's even a chance that the router could re-use the component. This sample stays with the observable paramMap strategy just in case.    
From <https://angular.io/guide/router#snapshot-the-no-observable-alternative>

~~~ typescript
constructor(private route: ActivatedRoute, ....)
...
this.myData = route..snapshot.data.myData;;
~~~

# Subscribe to Resolved Data in the Component

For details see also <https://angular.io/docs/ts/latest/guide/router.html#!#resolve-guard>

The `ActivatedRoute` object is injected into the constructor of the component. The data provided by the resolver can be retrieved in the component by subscribing to the `ActivatedRoute.data` property. 

~~~ typescript
constructor(private route: ActivatedRoute, ....)
...
route.data.subscribe((data: { myData: MyData }) => {
      // We do change the array, therefore copy it in other not change the original array
      let inputData: MyData = data.myData;
      ...
    });
~~~
{: .info title="Unsubscribe"}
When subscribing to an observable in a component, you almost always arrange to unsubscribe when the component is destroyed.    
There are a few exceptional observables where this is not necessary. The ActivatedRoute observables are among the exceptions.    
The ActivatedRoute and its observables are insulated from the Router itself. The Router destroys a routed component when it is no longer needed and the injected ActivatedRoute dies with it.    
Feel free to unsubscribe anyway. It is harmless and never a bad practice.    
See <https://angular.io/guide/router#observable-parammap-and-component-reuse>

{: .info title="Type of data object"}
The resolved data is provided by a parameter with the name 'data'. The resolved objects are nested in this object. Therefore we have to access the resolved data with the path prefix 'data.'. In the example the resolbed object is stored in the 'myData' property. Therefore we have to access is `data.myData`.  
Note that the structure of the data object is defined in the route definition.

In order to centralize knowledge about the resolved data structure, one can define constants and interfaces like

~~~ typescript
export const MY_RESOLVER = { myData: MyDataResolver };
export interface MyResolverResolvedData {
  myData: MyDataType;
}
~~~

and ues it in the route definition etc:

~~~ typescript
// route definition
{ path: 'myPath', 
  component: MyComponent, 
  resolve: MY_RESOLVER,
~~~

~~~ typescript
@NgModule({
  ...
  providers: [MY_RESOLVER.myData]
})
~~~

~~~ typescript
...
route.data.subscribe((data: MyResolverResolvedData) => {
      // We do change the array, therefore copy it in other not change the original array
      let inputData: MyData = data.myData;
      ...
    });
~~~


## Miscellaneous

### Access to URL Parameters

The simples way to accessed url parameters is via the `ActivatedRouteSnapshot` or `ActivatedRoute` objects. 

~~~ typescript
route: ActivatedRoute = ....
route.snapshot.params['myParam']
~~~

# References

* <https://angular.io/docs/ts/latest/guide/router.html>
