---
layout: default
title: Home
---

## My World

* [A Example Page](./pages/another-page.html).

## Post

{% for post in site.posts %}
	<h2>
        <a class="post-link" href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
    </h2>
{% endfor %}

## About Me
My name is Saikat Sharma. I study Computer Science and Engineering at Mawlana Bhashani Science and Technology University.
