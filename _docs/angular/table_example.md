---
title: Angular Table Example
tags: [angular]
category: angular
summary: "Angular table example with angular and bootstrap 4"
---

# Overview

# Paging

# Sorting

## The Table Sorter Component

The sorter "button" is a simple span element with some enhancements:

![](./table_example/table_sorter_component.png)

~~~html
<span role="button" (click)="switchSortOrder()">
<!-- Transclusion using ng-content -->
 <ng-content></ng-content>
  <i class="fa"  aria-hidden="true"
    [class.fa-sort-asc]="ascending && active"
    [class.fa-sort-desc]="!ascending && active" 
    [class.fa-sort]="!active"></i>
</span>
~~~
Here we use the `[role="button"] { cursor: pointer;}` property of bootstrap.
The Angular class binding (<https://angular.io/docs/ts/latest/guide/template-syntax.html#!#other-bindings>) sets different font awesome classes depending on the component properties `ascending` and `active`. 

If there is a click on the element, the on `(click)="switchSortOrder()` event binding calls the `switchSortOrder()` method of the component. 

~~~typescript
export class TableSorterComponent implements OnInit {

  /**
   * The object property path which is used for sorting. Can be nested path.
   */
  @Input('property')
  property: string;

  /**
   * Indicates whether the initial sorting should be done with this instance.
   */
  @Input('initialActive')
  initialActive = false;

  active = false;
  ascending = true;

  constructor(private eventService: TableSorterEventService) {
    // if another sorter was activated, this one should be deactivated.
    // This can be checked by comparing the property to sort:
    eventService.subscribe(e => this.active = e.property === this.property);
  }

  ngOnInit(): void {
    if (this.initialActive) {
      // triggers the initial sorting
      this.postEvent();
    }
  }

  switchSortOrder(): void {
    this.ascending = !this.ascending;
    this.postEvent();
  }

  private postEvent(): void {
    this.eventService.post(
      new TableSortChangeEvent(this.property,
        // the compare function to use for sorting.
        PoaUtils.withAscending(this.ascending, PoaUtils.getComparator(this.property))));
  }
}
~~~

## The TableSorterEventService

The comunication between the various `TableSorterComponent` instances and the host component is realized by a simple event service which is based on RxJs `Subject`: 

~~~typescript
@Injectable()
export class TableSorterEventService {

  private readonly subject = new Subject<TableSortChangeEvent>();
  constructor() {
  }

  subscribe(subscriber: (value: TableSortChangeEvent) => void) {
    return this.subject
      .subscribe(subscriber);
  }

  post(event: TableSortChangeEvent) {
    this.subject.next(event);
  }
}

export class TableSortChangeEvent {

  /**
   * The compareFunction is used to sort the data.
   */
  constructor(public readonly property: string, public readonly compareFunction: (a: any, b: any) => number) {

  }
}
~~~

Note that the `TableSortChangeEvent` has a function type property which is used as compare function when sorting the data.

## Use Table Sorter Component in a Component with a Table


~~~html
<table class="table  table-striped table-hover table-sm">
    <thead class="thead-default">
        <tr class="row">
          <!--  set the column width fixed. Has to be done also in the tbody area? -->
            <th class="col-sm-5">
                <poa-table-sorter property="longname">Name</poa-table-sorter>
            </th>
           ...
      </tr>
    </thead>
    <tbody>
        <tr *ngFor="let station of data" class="row">
            <td class="col-sm-5">{{station.longname}}</td>
            ,,,
        </tr>
    </tbody>
</table>
~~~


~~~typescript
@Component({
  ...
  providers: [TableSorterEventService]
})
export class StationsTableComponent {

  data: Station[] = [];
  
  private compareFunction: (a: Station, b: Station) => number;

  constructor(route: ActivatedRoute, eventService: TableSorterEventService) {
    // get resolved data via subscription
    route.data.subscribe((data: { stations: Station[] }) => {
      // We do change the array, therefore copy it in other not change the original array
      this.data = data.stations.slice(0);
      this.sortInputData();
    });
  
    // listen to changes in the TableSorterComponents: 
    eventService.subscribe(e => {
      this.compareFunction = e.compareFunction;
      this.sortData();
    });
  }
  
  private sortData(): void {
    if (this.compareFunction) {
      this.data.sort(this.compareFunction);
    }
  }
}
~~~

# References
* <https://v4-alpha.getbootstrap.com/>
* <http://fontawesome.io/>
* <https://angular.io/docs/ts/latest/guide/template-syntax.html#!#other-bindings>
