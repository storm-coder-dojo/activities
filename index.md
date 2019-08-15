---
---

## Python

  {% for doc in site.python %}
  {% if doc.category == "python" %}
  [{{ doc.title }}](/activities{{ doc.url }} "{{ doc.title }}")
  {% endif %}
  {% endfor %}

