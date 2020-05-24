---
layout: default
title: Home
---

## My World

* [A Example Page](./pages/another-page.html)

## Post

{% for post in site.posts %}
 [{{ post.title }}]({{ post.url | prepend: site.baseurl }}})
{% endfor %}

## About Me
My name is Saikat Sharma. I study Computer Science and Engineering at Mawlana Bhashani Science and Technology University.
