---
layout: page
title: Archive
---

### Blog
<ul class="archive-list">
{% for post in site..posts %}
  <li><a href="{{post.url}}">{{ post.title }}</a> - <span class="post-date">{{ post.date | date_to_string }}</span></li>
{% endfor %}
</ul>

### Writing
* [Pointers for Software Engineers](https://github.com/vicoyeh/pointers-for-software-engineers) - <span class="post-date">6 Jul 2020</span>
* [Moving a Business-Critical Monolith to Kubernetes](https://medium.com/blend-engineering/moving-a-business-critical-monolith-to-kubernetes-5180f465fdf) - <span class="post-date">4 Dec 2019</span>
* [Continuous delivery as code with Jenkins DSL](https://blend.com/blog/community/engineering/jenkins-dsl/) - <span class="post-date">26 Nov 2018</span>
