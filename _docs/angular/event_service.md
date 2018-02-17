---
title: Event Service with Decorator
tags: [angular]
category: angular
summary: "Event Service with Decorator"
---

# Overview

With the help of RxJS <http://reactivex.io/rxjs/> one can easily create a simple event service. The cumbersome part of using the event service is subscribing and ansubscribing to the observable in every class which should act as event listener. With the help of method decorators (<https://www.typescriptlang.org/docs/handbook/decorators.html>) one can mark a method as event listener. This works for objects which are managed by Angular and implement the lifecycle hooks `OnInit` `OnDestroy`. Note:

* `OnInit`: "Lifecycle hook that is called after data-bound properties of a directive are initialized." (
<https://angular.io/api/core/OnInit>)
* `OnDestroy`: Lifecycle hook that is called when a directive, pipe or service is destroyed. (
<https://angular.io/api/core/OnDestroy>)

This means that `OnInit` is not called for services.

# The Event Service

~~~ typescript
import {Injectable, OnDestroy} from '@angular/core';
import {Subscription} from 'rxjs/Subscription';
import {Subject} from 'rxjs/Subject';
import 'rxjs/add/operator/filter';

@Injectable()
export class EventService implements OnDestroy {

  private readonly subject = new Subject<any>();
  constructor() {
  }


  ngOnDestroy(): void {
    this.subject.complete();
  }

  subscribe<T>(eventType: {new(...args: any[]): T},
    next?: (value: any) => void, error?: (error: any) => void, complete?: () => void):
    Subscription {
    return this.subject.filter(e => e.constructor === eventType).subscribe(next, error, complete);
  }

  publish(event: any): void {
    this.subject.next(event);
  }
}
~~~

# The EventListener Decorator

~~~ typescript

import {EventService} from './event.service';
import {NgModule, getPlatform} from '@angular/core';
import {Subscription} from 'rxjs/Subscription';
import 'reflect-metadata';

@NgModule()
export class EventDecorator {

  static eventService: EventService;
  constructor(ev: EventService) {
    EventDecorator.eventService = ev;
  }
}


export function EventListener(): MethodDecorator {
  return function(target: any, propertyKey: string, descriptor: PropertyDescriptor) {
    // https://www.npmjs.com/package/reflect-metadata
    const fn = Reflect.getMetadata('design:paramtypes', target, propertyKey);

    if (fn && fn[0].prototype.constructor && target[propertyKey]) {
      const eventType = fn[0].prototype.constructor;
      const method = target[propertyKey];

      const ngOnInit = target.constructor.prototype.ngOnInit;
      //  let subscription: Subscription;
      target.constructor.prototype.ngOnInit = function() {
        const inj = getPlatform().bootstrapModule;
        // this ist z.B. die "component"
        this['subscription' + propertyKey] = EventDecorator.eventService.subscribe(eventType, method);
        // ursprüngliche Methode aufrufen nicht vergessen.
        if (ngOnInit) {
          ngOnInit.apply(this);
        }
      };

      // Wichtig: unsubscribe, sonst bleibt die Subscription, auch wenn die Komponente nicht mehr da ist
      const ngOnDestroy = target.constructor.prototype.ngOnDestroy;
      target.constructor.prototype.ngOnDestroy = function() {
        if (this['subscription' + propertyKey]) {
          this['subscription' + propertyKey].unsubscribe();
        }
        if (ngOnDestroy) {
          ngOnDestroy.apply(this);
        }
      };
    }
    return descriptor;
  };
}
~~~
# Usage

~~~ typescript

this.eventService.publish(new MyEvent());
~~~

Register a method of a class as event listener for the `MyEvent`:
~~~ typescript

@EventListener()
doSomething(x: MyEvent) {
   
}
~~~

# References

* <http://reactivex.io/rxjs/>
* <https://www.npmjs.com/package/reflect-metadata>
* <https://www.typescriptlang.org/docs/handbook/decorators.html>
