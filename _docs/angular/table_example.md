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

![](./table_example/table_sorter_component.png)

~~~html

The template for the 
<span role="button" (click)="switchSortOrder()">
<!-- Transclusion using ng-content -->
 <ng-content></ng-content>
  <i class="fa"  aria-hidden="true"
    [class.fa-sort-asc]="sortAscending && active"
    [class.fa-sort-desc]="!sortAscending && active" 
    [class.fa-sort]="!active"></i>
</span>
~~~


~~~typescript
export class TableSorterComponent implements OnInit {

  sortAscending = true;

  @Input('property')
  property: string;

  @Input('initialActive')
  initialActive = false;

  active = false;

  constructor(private eventService: TableSorterEventService) {
    eventService.subscribe(e => this.active = e.property === this.property);
  }

  ngOnInit(): void {
    if (this.initialActive) {
      this.postEvent();
    }
  }

  private postEvent(): void {
    this.eventService.post(new TableSortChangeEvent(this.property,
      PoaUtils.withAscending(this.sortAscending, PoaUtils.getComparator(this.property))));
  }

  switchSortOrder(): void {
    this.sortAscending = !this.sortAscending;
    this.postEvent();
  }
}
~~~


# References
* <https://v4-alpha.getbootstrap.com/>
* <http://fontawesome.io/>
