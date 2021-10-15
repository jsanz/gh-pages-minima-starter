---
title: "Tutorial VII: internal links"
date: 2020-04-19
layout: post
---

In your blog post you will write links to internal resources, like other articles, pages, and images. You can use relative paths for those assets so for example to link the archive you may use the following code `[archive](/archive)`. This will work fine, since that's the url of your archive, but what happens if you decide to change the url? You'd have to search for any occurrence in your site for any link and fix it.

A different approach is to use a Jekyll feature called *Liquid tags*. From the [documentation](https://jekyllrb.com/docs/liquid/):

> Jekyll uses the [Liquid](https://shopify.github.io/liquid/) templating language to process templates.
>
> Generally in Liquid you output content using two curly braces e.g. `{% raw %}{{ variable }}{% endraw %}` and perform logic statements by surrounding them in a curly brace percentage sign e.g. `{% raw %}{% if statement %}{% endraw %}`. To learn more about Liquid, check out the [official Liquid Documentation](https://shopify.github.io/liquid/).

There is a Liquid tag to link to files, getting as a result the corresponding url for the resource. So for our archive we would use:

```{% raw %}
[archive]({{ site.base_url }}{% link _pages/archive.md %})
{% endraw %}```

and this would be automatically converted into

```
[archive]({{ site.base_url }}{% link _pages/archive.md %})
```

For a blog post this:

```{% raw %}
[first post]({{ site.base_url }}{% link _posts/2020-03-18-my-first-post.md %})
{% endraw %}```

transforms into:

```
[first post]({{ site.base_url }}{% link _posts/2020-03-18-my-first-post.md %})
```

Apart from the benefit of being resilient to changes in the permalinks of your articles, this also has a very important effect: Jekyll **will fail** to process your site if you use a wrong link. This means that Jekyll checks your internal links on every built, and if for any reason you change the name of your file in the future, Jekyll will error with details on where the offending link lives.

An important final note is that if you paid attention, the links are relative to the root of your domain. If you deploy this site in your personal GitHub space that's OK but if you publish it in a project so your URL is like `https://yourusername.github.io/project` then the URLs will fail. You'd need to put `{% raw %}{{ site.base_url }}{% endraw %}` **before** the `link` tag, for absolute URLs that point to the correct resource.

More details on this [nice article](https://www.webisland.agency/blog/jekyll-internal-links/).
