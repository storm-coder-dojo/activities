---
---

## Python

  {% for doc in site.python %}
  {% if doc.category == "python" %}
  [{{ doc.title }}]({{ doc.url }} "{{ doc.title }}")
  {% endif %}
  {% endfor %}

