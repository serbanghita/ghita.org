---
layout: home
title: Home
---

# Welcome to Ghita.org

This is a minimal Jekyll website powered by the [no-style-please](https://github.com/riggraz/no-style-please) theme.

## About

This site demonstrates a clean, minimalist approach to web design with:
- A homepage
- A contact page
- Blog posts

Check out the [blog posts](/blog) or visit the [contact page](/contact) to get in touch.

## Latest Posts

{% for post in site.posts limit:3 %}
- [{{ post.title }}]({{ post.url }}) - {{ post.date | date: "%B %d, %Y" }}
{% endfor %}

[View all posts →](/blog)
