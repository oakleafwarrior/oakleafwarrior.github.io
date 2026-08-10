---
title: Blog
permalink: /blog/
description: Notes and longer write-ups.
---

<!-- Optional one-line description of what you write about. -->

<ul class="post-list">
  {%- for post in site.posts %}
  <li>
    <a class="title" href="{{ post.url | relative_url }}">{{ post.title }}</a>
    <span class="post-meta">{{ post.date | date: "%B %-d, %Y" }}</span>
    {%- if post.summary or post.description %}
    <span class="summary">{{ post.summary | default: post.description }}</span>
    {%- endif %}
  </li>
  {%- else %}
  <li class="muted">No posts yet.</li>
  {%- endfor %}
</ul>

<p class="muted"><a href="{{ '/feed.xml' | relative_url }}">RSS feed</a></p>
