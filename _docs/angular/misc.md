---
title: Diverse Angular Notes
tags: [angular]
category: angular
summary: "Diverse Notes about Angular 2"
---

Some unrelated stuff

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


~~~ts
export class NavigationLoadingOverlayComponent implements OnDestroy {

  loading = false;

  private counter = 0;

  private subscription: Subscription;
  constructor(router: Router) {
    router.events.subscribe((event: Event) => {
      this.navigationInterceptor(event);
    });
  }

  // Shows and hides the loading spinner during RouterEvent changes
  navigationInterceptor(event: Event): void {
    // http://stackoverflow.com/questions/37069609/show-loading-screen-when-navigating-between-routes-in-angular-2
    if (event instanceof NavigationStart) {
      // the counter is an indicator that we are between start and stop loading (counter > 0)
      this.counter++;
      // the loading should be set with a small time delay
      setTimeout(() => {
        // if counter === 0, there was a NavigationEnd event (etc) meanwhile -> do not show overlay
        if (this.counter > 0 && !this.loading) {
          this.loading = true;
        }
      }, 100);
  }
    // do not forget cancel and error case!
    if (event instanceof NavigationEnd || event instanceof NavigationCancel || event instanceof NavigationError) {
      if (this.counter > 0) {
        this.counter--;
      }
      if (this.counter === 0 && this.loading) {
        this.loading = false;
      }
    }
    // the RoutesRecognized event does nothing
  }
  ngOnDestroy(): void {
    this.subscription.unsubscribe();
  }
}
~~~
