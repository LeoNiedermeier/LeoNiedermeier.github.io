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


## Backup
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

# Site info


Generatd: {{ site.time | date_to_rfc822 }}