---
layout: default
---

#### 今年の記事

<ul>
<!-- {\% for post in site.posts reversed %} -->
{% for post in site.posts %}
    {% capture year %}{{ post.date | date: "%Y" }}{% endcapture %}
    {% assign currentYear = 'now' | date: "%Y" %}
    {% if currentYear == year %}
      <li>{{ post.date | date: "%m 月 "}}<a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
{% endfor %}
</ul>

<br/>

### [全ての記事があるコーナー](/activities)

<br/>

---

<br/>

### [LT など登壇資料コーナー](/slides)

---

<br/>

### Tweet コーナー

<div style="max-width:400px;margin:0 auto">
  <a class="twitter-timeline" data-height="600" data-theme="light" href="https://twitter.com/takapiro_99?ref_src=twsrc%5Etfw">Tweets by takapiro_99</a>
  <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
</div>

---

#### [sandbox page](/sandbox)

#### Other Links

- [制作物まとめ Gist](https://gist.github.com/takapiro99/5598b0f5c730e47d4b58c64c12011297)
- [Wantedly](https://www.wantedly.com/id/takapiro99)
- [linkedIn](https://www.linkedin.com/in/takapiro99/)
