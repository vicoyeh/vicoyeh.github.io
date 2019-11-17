---
layout: page
title: Archive
---

### Blog
<ul class="archive-list">
{% for post in site..posts %}
  <li><a href="{{post.url}}">{{ post.title }}</a> - <span class="post-date" style="display: inline-block">{{ post.date | date_to_string }}</span></li>
{% endfor %}
</ul>

### Writing
* [Continuous delivery as code with Jenkins DSL](https://blend.com/blog/community/engineering/jenkins-dsl/)

### Talks
* tbd