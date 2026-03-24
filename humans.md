---
layout: default
title: Humans
---

# Humans

I have added a [`human.json` file]({% link _posts/2026-03-20-human.md %}) to this website to vouch for the websites and blogs of people I know to be human. Here is a list of those links in a nice browser-friendly format:

{% for human in site.data.humans %}
  * [{{ human.url }}]({{ human.url }})
{%- endfor -%}
<br>

[You can view the actual `human.json` file here.]({% link human.json %})
