## Sobre tecnología e internet

Traduciendo el mundo de Internet al idioma más positivo y no lo digo yo.
[Human language reveals a universal positivity bias](https://www.pnas.org/content/112/8/2389)

### Últimas entradas
{% for post in site.posts limit:5 %}
* [{{ page.date | date_to_xmlschema }} - {{ post.title }}]({{ post.url }})
{% endfor %}
