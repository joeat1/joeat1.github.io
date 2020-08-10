---
layout: page
title: About
description: 改变世界
keywords: 程序员
comments: true
menu: 关于
permalink: /about/
---

一切随缘。

## 联系

<ul>
{% for website in site.data.social %}
<li>{{website.sitename }}：<a href="{{ website.url }}" target="_blank">@{{ website.name }}</a></li>
{% endfor %}
</ul>
