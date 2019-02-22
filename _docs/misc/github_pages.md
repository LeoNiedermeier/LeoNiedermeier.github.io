---
title: Notes about Github Pages
tags: [misc]
category: misc
summary: ""
---
# Kramdown Syntax

<https://kramdown.gettalong.org/quickref.html>

# Local Extensions 


# Panels
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

Code Block:

~~~~ markdown
{: .code title="mycode title"}
~~~css
[data-toggle='collapse'] .toggler::before {
	content: '\f077';
}
~~~
~~~~

{: .code title="mycode title"}
~~~css
[data-toggle='collapse'] .toggler::before {
	content: '\f077';
}
~~~

## Collapsible Panel
Same as panel wih an additional `.x`:

~~~  markdown
{: .success title="My Collapsible Panel" .x}
Some text
~~~

{: .success title="My Collapsible Panel" .x}
Some text

# HTML in Markdown Page

HTML can be uses in the markdown, see <https://kramdown.gettalong.org/syntax.html#html-blocks>

If the content of the html should be parsed as marksown, add a `markdown=1` attribute.


~~~ html
<div class="danger x" title="My Danger Title" markdown="1">
...
...
</div>
~~~

Example with Code:

<div class="danger x" title="My Danger Title" markdown="1">
Embedded code block
~~~ java
String x = "ABC";
~~~
...
</div>

 
# Misc

Does not work in IE:

~~~html
<details markdown="1">
  <summary>With Code Block</summary>
  # some text
</details>
~~~

Rendered:

<details markdown="1">
  <summary>With Code Block</summary>
# some text
</details>

# Errors Syntax Highlighting

Problem: Html code with angular elements is not valid html and therefore the syntax highlighter marks these elements as invalid (red).

Solution: remove the `.highlight .err` from the file `syntax.css`

~~~ css
.highlight .c { color: #999; } /* Comment */
//.highlight .err { color: #a00; background-color: #faa } /* Error */
.highlight .k { color: #069; } /* Keyword */
~~~

# Site info


Generatd: {{ site.time | date_to_rfc822 }}

# Backup

With a small javascript the to the jekyll css classes bootstrap classes are added:

~~~ javascript
$(document).ready(
  function() {
    $(".success").addClass("alert alert-success");
    $(".info").addClass("alert alert-info");
    $("table").addClass("table table-bordered table-striped");
});
~~~

Collapse behaviour with `.x` is added by a littele javascript:
~~~javascript
$('p[title], div[title]').each(function () {
        if ($(this).hasClass('x')) {
          var randomid = 'collapse_' + (collapse_id_counter++);
          $(this).wrapInner('<div class="collapse-inner collapse" id="' + randomid + '"></div>').prepend(function () {
            return '<div class="font-weight-bold collapsed" style="cursor: pointer"  data-toggle="collapse" data-target="#' + randomid + '">'
              + "<i class='toggler fa mr-2'></i>" + $(this).attr('title') + "</div></div>";
          });
        }
        else {
          $(this).wrapInner('<div class="no-collapse-inner"></div>')
            .prepend(function () { return '<div class="font-weight-bold">' + $(this).attr('title') + '</div></div>'; });
        }
      });
~~~

## Title Capitalization

<https://capitalizemytitle.com/>
