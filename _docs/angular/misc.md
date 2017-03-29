---
title: Diverse Angular Notes
tags: [angular]
category: angular
summary: "Diverse Notes about Angular 2"
---

Some unrelated stuff

# Overlay

Oberlay with Font Awedome and Bootstrap 4. Note that the overlay is always shown. (The bootstrap examples show the overlay on button click).

~~~html
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
