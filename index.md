<html>
    <h2>{{ site.data.navigation.docs_list_title }}</h2>
    <ul>
      {% for item in site.data.navigation.docs %}
        <li><a href="{{ item.link }}">{{ item.name }}</a></li>
      {% endfor %}
    </ul>
</html>

## Welcome!

Welcome to my personal site. As you can see, it is still under construction, but fill free to look at the posts in the blog section.

Cheers.
Héctor
