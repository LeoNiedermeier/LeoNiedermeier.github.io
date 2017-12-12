---
title: Errors and Fixes
tags: [angular]
category: angular
summary: "Errors and Fixes"
---

~~~
/home/dev/angular/iex-example/src/app/stock/news/stock-news-resolver.service.ts (19,5): Type 'Observable<{} | StockNewsItem[]>' is not assignable to type 'Observable<StockNewsItem[]>'.
  Type '{} | StockNewsItem[]' is not assignable to type 'StockNewsItem[]'.
    Type '{}' is not assignable to type 'StockNewsItem[]'.
      Property 'includes' is missing in type '{}'.
~~~

Code: 

~~~typescript
resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<Array<StockNewsItem>> {
    const sym = route.parent.paramMap.get('symbol');

    return this.iexService.stockNews(sym).catch(error => {
      console.log(`Retrieval error: ${error}`);
      return Observable.of(new Array<StockNewsItem>());
    });

  }
~~~

Problem / Solution: forgot import of "of": `import 'rxjs/add/observable/of';`
