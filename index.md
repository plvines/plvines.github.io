---
layout: default
---

![](assets/images/headshot.png){:height="256px"}

# [](#header-1)Summary:

I am a Principal Cybersecurity Research Engineer at BAE Systems with broad interests across the spectrum of security and privacy. 

I received my PhD from the University of Washington's Allen School of Computer Science & Engineering in 2017 where my thesis explored new digital surveillance methods and countermeasures.

# [](#header-1)Thoughts:

{% for post in site.posts %}
  <article class="{% if forloop.first %}first{% elsif forloop.last %}last{% else %}middle{% endif %}">
                    <div class="article-head">
                                <h2 class="title"><a href="{{ post.url }}" class="js-pjax">{{ post.title }}</a></h2>
                                                               <p class="date">{{ post.date | date: "%b %d, %Y" }}</p>
                                                                                  </div><!--/.article-head-->
                                                                                        <div class="article-content">
                                                                                               {{ post.long_description }}
  <a href="{{ post.url }}" class="full-post-link js-pjax">Read more</a>
  </div><!--/.article-content--></article>
  {% if forloop.last %}{% else %}<div class="separator"></div>{% endif %}
  {% endfor %}