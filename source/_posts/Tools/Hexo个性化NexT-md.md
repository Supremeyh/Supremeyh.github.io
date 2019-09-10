---
title: Hexo个性化NexT
date: 2019-01-25 22:35:39
categories: Tools
tags: ['Tools', 'Hexo']
---


### 常见问题
* 部署到Github Pages 打开categories出现 404
原因：文件夹大小写问题导致的，生成Category文件夹时大写，之后改成小了写。git 默认忽略文件名大小写，所以即使文件夹大小写变更，git 也检测不到。
方案：1、进入到博客项目中 .deploy_git文件夹，修改 .git 下的 config 文件，将 ignorecase=true 改为 ignorecase=false。 2、删除博客项目中 .deploy_git 文件夹下的所有文件，并 push 到 Github 上, 这一步是清空你的 github.io 项目中所有文件。3、重新生成部署 hexo g -d

* 打开博客很慢
原因：加载fonts.googleapis.com字体库导致
方案：在NexT主题的_config.yml里面的host: 改为host: //fonts.lug.ustc.edu.cn 。 (中科大的源)

* 设置内容隐藏不显示
post 头部标识 hide: true

修改主题配置项
```js
// Hexo\themes\next\layout\index.swig
{% block content %}
  <section id="posts" class="posts-expand">
    {% for post in page.posts %}
        {% if post.hide != true %}
            {{ post_template.render(post, true) }}
        {% endif %}
    {% endfor %}
  </section>

  {% include '_partials/pagination.swig' %}
{% endblock %}
```





