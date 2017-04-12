---
title: Unsubscribing from Subscription
tags: [angular]
category: angular
summary: "When and how to unsibscribe from a subscription"
---

# Unsubscribing from Subscription

## Overview

Subscribing to an `Observable` is often made via an inner class or closure like:

~~~typescript
myObservable.subscribe(things => console.log(things));
~~~

Depending on the lifecycle of the objects, the references can be garbage collected or not. If the surrounding object, 
for instance a angular component, is not longer used, it could be garbage collected. But if this object is transitively referenced
in a subscription (see above), the object can not be garbage collected. Worst it is maybe not longer reachable by code.

The need for unsubscribing depends on

* Lifetime of objects: If the subscriber has a shorter lifetime as the `Observable`, then unsubscribe
* Infiniteness of sequence: If the `Observable` produces a finite sequence and calls `complete` or `error`, no need for unsubscribe

## Solution

See <https://angular.io/docs/ts/latest/guide/router.html>

> When subscribing to an observable in a component, you almost always arrange to unsubscribe when the component is destroyed.
> There are a few exceptional observables where this is not necessary. The ActivatedRoute observables are among the exceptions.
> The ActivatedRoute and its observables are insulated from the Router itself. The Router destroys a routed component when it is no longer needed and the injected ActivatedRoute dies with it.
> Feel free to unsubscribe anyway. It is harmless and never a bad practice.

and <http://stackoverflow.com/questions/38008334/angular2-rxjs-when-should-i-unsubscribe-from-subscription>

~~~typescript

import 'rxjs/add/operator/takeUntil';

... implements OnDestroy, OnInit {
    private ngUnsubscribe: Subject<void> = new Subject<void>();

   
    ngOnInit() {
        this.myThingService.getThings()
              // this is important:
            .takeUntil(this.ngUnsubscribe)
            .subscribe(things => console.log(things));
    }

    ngOnDestroy() {
        this.ngUnsubscribe.complete();
    }
}
~~~
## References

* <https://angular.io/docs/ts/latest/guide/router.html>
* <http://stackoverflow.com/questions/38008334/angular2-rxjs-when-should-i-unsubscribe-from-subscription>
* <http://brianflove.com/2016/12/11/anguar-2-unsubscribe-observables/>
* <https://auth0.com/blog/four-types-of-leaks-in-your-javascript-code-and-how-to-get-rid-of-them/>
