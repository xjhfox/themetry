---
layout: post
title:  "jekyll中如何不解析liquid并显示"
date:   2017-03-16 19:05:13 +0000
category: tech
---
{% assign openTag = '{%' %}

因为liquid会被自动解析,所以要显示必须要写在
{% raw %}
`{% raw %}`
和
{% endraw %}{{ openTag }} endraw %}{% raw %}
两者之间

**比如**:

`{% raw %}`

{% include intensedebate-comments.html %}

{% endraw %}{{ openTag }} endraw %}{% raw %}

{% endraw %}

会显示为
{% raw %}
	{% include intensedebate-comments.html %}
{% endraw %}

但是因为无法嵌套,所以显示
{% raw %}
`{% raw %}`和{% endraw %}{{ openTag }} endraw %}{% raw %}本身需要特殊处理下.
{% endraw %}可以参看[这里][how]


---
{% if site.intensedebate_comments %}
  {% include intensedebate-comments.html %}
{% endif %}

[how]: http://blog.slaks.net/2013-06-10/jekyll-endraw-in-code/

