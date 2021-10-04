<!-- {{site.baseurl}}はレポジトリの設定によっては/sample...で上手く出来ない時があるので必要 -->

# takapiro99 のページ

---

<br/>

#### 今年の記事

<ul> <!-- 2021年の記事一覧 -->
{% for post in site.posts reversed %}
    {% capture year %}{{ post.date | date: "%Y" }}{% endcapture %}
    {% assign currentYear = 'now' | date: "%Y" %}
    {% if currentYear == year %}
      <li>{{ post.date | date: "%m 月 "}}<a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
{% endfor %}
</ul>

<br/>

### [全ての記事を見る](/activities)

<br/>

---

<a class="twitter-timeline" data-height="600" data-theme="light" href="https://twitter.com/takapiro_99?ref_src=twsrc%5Etfw">Tweets by huitgroup</a> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
