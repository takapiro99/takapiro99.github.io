---
layout: default
title: 活動記録一覧
---

<p class="end-of-content"><a href="/">←&nbsp;Home</a></p>

# 全ての記事

---

<ul>
{% for post in site.posts  %}

  {% if post.next %}
  {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
  {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
  {% endif %}

  {% if post.next %}
    {% if year != nyear %}
      <!-- 新しい年だったとき -->
      </ul>
      <li><h3 id="{{ post.date | date: '%Y' }}">{{ post.date | date: '%Y' }} 年</h3></li>
      <ul>
        <li>{{ post.date | date: "%m月" }} <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% else %}
      <!-- 前の記事と同じ年だったとき -->
      <li>{{ post.date | date: "%m月" }} <a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% else %}
    <!-- nyearが無いとき、つまり始めの年のとき -->
    <li><h3 id="{{ post.date | date: '%Y' }}">{{ post.date | date: '%Y' }} 年</h3></li>
    <ul>
      <li>{{ post.date | date: "%m月" }} <a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endif %}

  {% if forloop.last %}</ul>{% endif %}

{% endfor %}
</ul>

<style>
  li { margin: 10px 0; }
</style>