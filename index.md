---
layout: default
title: How to Use R
author: Marta
published: 2019-05-19
---

# How to use R



## Tutorial pages
{% assign tuts = site.tutorials | sort:"weight" %}
<ul>
{% for tutorial in tuts %}
<li><a href="{{site.baseurl}}{{ tutorial.url }}">{{ tutorial.title }}</a></li>
{% endfor %}
</ul>

This guide is released as CC0. Some contents will have retained rights.
