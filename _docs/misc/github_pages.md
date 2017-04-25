---
title: Notes about Github Pages
tags: [misc]
category: misc
summary: ""
---


# Kramdown Syntax

<https://kramdown.gettalong.org/quickref.html>

# Local Extensions 

With block attributes (<https://kramdown.gettalong.org/quickref.html#block-attributes>) we create custom styling:

~~~ markdown
{: .info title="My Title"}
Some text
~~~

{: .info title="My Title"}
Some text


~~~  markdown
{: .success title="My Title"}
Some text
~~~

{: .success title="My Title"}
Some text

~~~  markdown
{: .danger title="My Title"}
Some text
~~~

{: .danger title="My Title"}
Some text


# HTML in Markdown Page

HTML can be uses in the markdown, see <https://kramdown.gettalong.org/syntax.html#html-blocks>

If the content of the html should be parsed as marksown, add a `markdown=1` attribute.


~~~html
<div class="danger" title="My Danger Title" markdown="1">
...
...
</div>
~~~

Example with Code:

<div class="danger" title="My Danger Title" markdown="1">
...
~~~html
<div class="danger" title="My Danger Title" markdown="1">
~~~
...
</div>

# Panel Header

<div class="panel panel-default">
  <div class="panel-heading">
    <h3 class="panel-title">Panel title</h3>
  </div>
  <div class="panel-body" markdown="1">

~~~html
<div class="danger" title="My Danger Title" markdown="1">
~~~

</div>
</div>


# Site info


Generatd: {{ site.time | date_to_rfc822 }}

# Backup
The title is set by css:

~~~ css
.success::before, .info::before {
	content: attr(title);
	display: block;
	font-weight: bold;
}
~~~

With a small javascript the to the jekyll css classes bootstrap classes are added:

~~~ javascript
$(document).ready(
  function() {
    $(".success").addClass("alert alert-success");
    $(".info").addClass("alert alert-info");
    $("table").addClass("table table-bordered table-striped");
});
~~~
