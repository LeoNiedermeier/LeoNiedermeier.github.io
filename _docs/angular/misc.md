---
title: Diverse Angular Notes
tags: [angular]
category: angular
summary: "Diverse Notes about Angular 2"
---

Some unrelated stuff

# Unsubscribing from Subscription

See <https://angular.io/docs/ts/latest/guide/router.html>

> When subscribing to an observable in a component, you almost always arrange to unsubscribe when the component is destroyed.
> There are a few exceptional observables where this is not necessary. The ActivatedRoute observables are among the exceptions.
> The ActivatedRoute and its observables are insulated from the Router itself. The Router destroys a routed component when it is no longer needed and the injected ActivatedRoute dies with it.
> Feel free to unsubscribe anyway. It is harmless and never a bad practice.

and <http://stackoverflow.com/questions/38008334/angular2-rxjs-when-should-i-unsubscribe-from-subscription>

~~~typescript
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


# Overlay

Oberlay with Font Awesome and Bootstrap 4. Note that the overlay is always shown. (The bootstrap examples show the overlay on button click).

~~~html
<!-- load css from cdns: -->
<link href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0-alpha.6/css/bootstrap.min.css" rel="stylesheet">
<link href="https://maxcdn.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet">

...
<!-- bootstrap 4 modal: maybe it can be made by less classes -->
<!-- the background: rgba(0,0,0,0.5); prohibits inheritance of opacity to spinner -->
<div *ngIf="loading" class="modal-content modal-backdrop justify-content-center align-items-center w-100 h-100 " 
                     style="background: rgba(0,0,0,0.5);">
  <!-- animated spinner -->
  <i class="fa fa-spinner fa-spin fa-3x fa-fw text-white"></i>
  <span class="sr-only">Loading...</span>
</div>
~~~
