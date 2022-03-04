---
title: "Tutorial VIII: HTML"
date: 2020-04-20
layout: post
---

Quick note on highlighting that Markdown supports raw HTML. That means that if for some reason Markdown falls short or becomes inconvenient for a thing you are authoring, you can always use HTML as a fallback solution. One very common scenario is customizing the width and height of your images.

You can check the source code of the [editing content]({% link _posts/2020-03-24-edit-content.md %}) post to see how I moved from the standard syntax for images using `![alt text](image path)` to use HTML to allow to set up a custom width using a CSS style.

```html{% raw %}
<div style="text-align:right; margin: 20px 0">
  <p>
    Another example could be to 
    <span style="color:red">change text color</span>.
  </p>
</div>
{% endraw %}```


<div style="text-align:right; margin: 20px 0">
<p>
Another example could be to <span style="color:red">change text color</span>.
</p>
</div>

Easy right? 😀