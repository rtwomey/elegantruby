---
layout: page
title: ElegantRuby.com
tagline: Writing elegant ruby code.
---
{% include JB/setup %}

<div class="row">
  <div class="span8">
    {% for post in site.posts %}
      <h2><a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></h2>

      {{ post.content }}

      <hr/>

      {% include post_footer %}

      <hr/>

      <br/>
    {% endfor %}
  </div>

  <div class="span4">
    {% include sidebar %}
  </div>
</div>
