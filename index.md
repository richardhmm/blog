---
layout: home
---

<div class="index-content blog">
    <div class="section">
        <ul class="artical-cate">
            <li class="on"><a href="/blog/"><span>文章</span></a></li>
            <li style="text-align:center"><a href="/blog/aboutme"><span>关于我</span></a></li>
            <li style="text-align:right"><a href="/blog/archive"><span>归档</span></a></li>
        </ul>

        <div class="cate-bar"><span id="cateBar"></span></div>

        <ul class="artical-list">
        {% for post in site.categories.blog %}
            <li>
                <h2><a href="/blog{{ post.url }}">{{ post.title }}</a></h2>
                <div class="title-desc">{{ post.description }}</div>
            </li>
        {% endfor %}
        </ul>
		<div class="container" id="cc">
			<p>&nbsp;&nbsp;模板来自<a href="http://beiyuu.com/">BeiYuu</a>博客 Rebuild by richardhmm 2014.</p>
		</div>
    </div>
    <div class="aside">
    </div>
</div>
