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

      <div class="row">
        <div class="span6">
          <p>
            Posted on {{ post.date | date_to_string }} in <a href="/categories.html#{{ post.category }}-ref">{{ post.category }}</a>.<br/>
            Tagged:

            {% assign last_item = (post.tags | last) %}
            {% for tag in post.tags %}
              <a href="/tags.html#{{ tag }}-ref">{{ tag }}</a>{% unless tag contains last_item %},{% endunless %}
            {% endfor %}<br/>

          </p>
        </div>

        <div class="span2">
          <p>
            <a href="{{ BASE_PATH }}{{ post.url }}">Read more...</a>
          </p>
        </div>
      </div>

      <hr/>

      <br/>
    {% endfor %}
  </div>

  <div class="span4">
    {% include sidebar %}
  </div>
</div>
