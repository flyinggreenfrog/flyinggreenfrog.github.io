---
title: Knowledgebase
last-changed: (2018-05-02)
navigation-order: 10
---
{%- for site-category in site.knowledgebase-categories -%}
## {{ site-category }}
<ul>
  {%- for page in site.pages -%}
    {%- if page.knowledgebase == true -%}
      {%- for page-category in page.categories -%}
        {%- if page-category == site-category %}
  <li><a href="{{ page.url }}">{{ page.title }}</a> <small>{{ page.last-changed }}</small></li>
        {%- endif -%}
      {%- endfor -%}
    {%- endif -%}
  {%- endfor %}
</ul>
{%- endfor -%}
