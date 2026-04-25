---
title: Blog
nav_order: 2
---

# Posts

최신 글부터 정리합니다.

{% assign sorted_posts = site.posts | sort: 'date' | reverse %}
{% for post in sorted_posts %}
## [{{ post.title }}]({{ post.url | relative_url }})

{{ post.date | date: "%Y-%m-%d" }}

{% if post.excerpt %}
{{ post.excerpt | strip_html | truncate: 180 }}
{% endif %}

{% endfor %}
