---
title: Event Service with Decorator
tags: [angular]
category: angular
summary: "Event Service with Decorator"
---

# Overview


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

# References
