---
title: 书籍
date: 2023-10-01

---

这里是所有书籍的列表：

{% for post in site.posts %}
  <!-- 调试信息：输出所有文章的标题和分类 -->
  <p>文章标题: {{ post.title }}, 分类: {{ post.categories }}</p>
  {% if '书籍' in post.categories %}
    - [{{ post.title }}]({{ post.url }})
  {% endif %}
{% endfor %}