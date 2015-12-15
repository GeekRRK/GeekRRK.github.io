---
layout: post
title: Blog with Jekyll
---

###1.
`$ mkdir Blog` <br />
`$ cd Blog` <br />
`$ git init` <br />
`$ git checkout --orphan gh-pages`

###2.
Create a file named _config.yml, which is the configuration of the project, in directory Blog/ and add `baseurl: /Blog` to it. [Other configs](http://jekyllrb.com/docs/configuration/).

###3.
Create a directory named _layouts, which stores templates, in directory Blog/. Enter directory _layouts/ and create a file named default.html as the default template of the project then add the following content to it.

```html
{% raw %}
<!DOCTYPE html>
<html>
<head>
<meta http-equiv="content-type" content="text/html; charset=utf-8" />
<title>{{ page.title }}</title>
</head>
<body>
{{ content }}
</body>
</html>
{% endraw %}
```

Jekyll use [Liquid](https://github.com/shopify/liquid/wiki/liquid-for-designers) as it's template language. [See more variables](http://jekyllrb.com/docs/variables/).

###4.
Create a directory named _posts, which stores blogs. Enter this directory and create a blog named `Y-M-D-Title.html(md)` then add the following content to it.

```html
{% raw %}
---
layout: default
title: Blog with Jekyll
---
<h2>{{ page.title }}</h2>
<p>The steps to blog with Jekyll.</p>
<p>{{ page.date | date_to_string }}</p>
{% endraw %}
```

###5.
Create index.html in the directory blog/ and add the following content to it.

```html
{% raw %}
---
layout: default
title: Blog of Al Young
---
<h2>{{ page.title }}</h2>
<ul>
{% for post in site.posts %}
<li>
{{ post.date | date_to_string }} <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
</li>
{% endfor %}
</ul>
{% endraw %}
```

###6.
`$ git add .` <br />
`$ git commit -m "first post"` <br />
`$ git remote add origin https://github.com/GeekRRK/Blog.git` <br />
`$ git push origin gh-pages` 

###7.
Copy some css code into pygment_trac.css which make the code snippets highlited and style.css which make the text look better.
