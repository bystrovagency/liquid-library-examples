# Blog 

 ## Table of contents
1. [Blog article list](#blog-article-list) 
2. [Blog article page](#blog-article-page) 
3. [Blog tag listing](#blog-tag-listing) 
4. [Comment list](#comment-list) 
5. [Featured blog posts](#featured-blog-posts) 
6. [Previous and next article buttons](#previous-and-next-article-buttons) 
 
 
------
### <a name="blog-article-list">Blog article list</a>
Blogs posts are a standard element of most websites. This example outputs a list of blog posts associated with a specific blog. The Liquid and HTML needed to display article titles, featured images, article excerpts, article tags, authors, and dates are found in this component. Learn more about blog templates in the [Shopify Help Center](https://help.shopify.com/en/themes/development/templates/blog-liquid).

1.  Add the following code to the file that outputs content for the `blog` page.
2.  Adjust styling on the theme's stylesheet for appropriate positioning and formatting.

```liquid
<h1>{{ page_title }}</h1>

<ul>
  {%- for article in blog.articles -%}
    <li>

      <h2>
        <a href="{{ article.url }}">{{ article.title }}</a>
      </h2>

      {%- if section.settings.blog_show_author -%}
        <span>
          By {{ article.author }}
        </span>
      {%- endif -%}

      {%- if section.settings.blog_show_date -%}
        <span>
          {{ article.published_at | time_tag: format: 'month_day_year' }}
        </span>
      {%- endif -%}

      {%- if article.image -%}
        <a href="{{ article.url }}">
          <img src="{{ article.image | img_url: '300x300' }}" alt="">
        </a>

        <noscript>
          <p>
            <a href="{{ article.url }}">
              {{ article | img_url: '455x300', scale: 2 | img_tag: article.title }}
            </a>
          </p>
        </noscript>
      {%- endif -%}

      <p>
        {%- if article.excerpt.size > 0 -%}
          {{ article.excerpt }}
        {%- else -%}
          {{ article.content | strip_html | truncate: 150 }}
        {%- endif -%}
      </p>

      {%- if article.tags.size > 0 -%}
        <ul>
          {{ 'blogs.article.posted_in' }}
            <li>
              {%- for tag in article.tags -%}
                <a href="{{ blog.url }}/tagged/{{ tag | handle }}">{{ tag }}</a>{% unless forloop.last %}, {% endunless %}
              {%- endfor -%}
            </li>
        </ul>
      {%- endif -%}

      <ul>
        <li>
          <a href="{{ article.url }}" aria-label="Read more: {{ article.title }}">
            Read more
          </a>
        </li>
        {%- if blog.comments_enabled? and article.comments_count > 0 -%}
          <li>
            <a href="{{ article.url }}#comments">
              {{ article.comments_count }}
            </a>
          </li>
        {%- endif -%}
      </ul>

    </li>
  {%- endfor -%}
</ul>

{% schema %}
{
  "name": "Blog pages",
  "settings": [
    {
      "type": "checkbox",
      "id": "blog_show_author",
      "label": "Show author",
      "default": true
    },
    {
      "type": "checkbox",
      "id": "blog_show_date",
      "label": "Show date",
      "default": true
    }
  ]
}
{% endschema %}
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=images&type=Code}">`#images`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= pages&type=Code}">`#pages`</a> 

------
### <a name="blog-article-page">Blog article page</a>
The blog article page is a dedicated page for an individual blog article, or post. It includes elements such as the article title, author, published date, content, and tags. In this example, the visibility of the article author and published date can be enabled or disabled from the [theme editor](https://help.shopify.com/en/themes/development/theme-editor).

1.  Place the following code in the `article-template.liquid` section file. If this file doesn't exist, add a new section called `article-template`.
2.  Make sure that the `article.liquid` file includes the following Liquid tag: `{% section 'article-template' %}`. Add this tag to the file if it doesn’t already exist.
3.  Additional elements, such as social sharing buttons, can be added if required.

```liquid
<article>

  <header>
    <h1>{{ article.title }}</h1>

    {%- if section.settings.blog_show_author -%}
      <span>
        By {{ article.author }}
      </span>
    {%- endif -%}

    {%- if section.settings.blog_show_date -%}
      <span>
        {{ article.published_at | time_tag: format: 'month_day_year' }}
      </span>
    {%- endif -%}

  </header>

  {{ article.content }}

  {%- if article.tags.size > 0 -%}
    <footer>
      <ul aria-label="Tags">
      {%- for tag in article.tags -%}
        <li>
          <a href="{{ blog.url }}/tagged/{{ tag | handle }}" class="article__grid-tag">
            {{ tag }}
          </a>
        </li>
      {%- endfor -%}
      </ul>
    <footer>
  {%- endif -%}

</article>

{% schema %}
{
  "name": "Posts",
  "settings": [
    {
      "type": "checkbox",
      "id": "blog_show_author",
      "label": "Show author",
      "default": true
    },
    {
      "type": "checkbox",
      "id": "blog_show_date",
      "label": "Show date",
      "default": true
    }
  ]
}
{% endschema %}
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=page&type=Code}">`#page`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= article&type=Code}">`#article`</a> 

------
### <a name="blog-tag-listing">Blog tag listing</a>
Tags are a type of taxonomy, and are often used to reflect the keywords of a blog article. Tags also provide a means of navigation for customers browsing for similar blog posts. This component displays all the tags associated with a store, including the tags of articles that are not in the current pagination view.

1.  Locate the section file which contains your theme's blog article listing, such as `blog-template.liquid`, and paste the code below into the file.
2.  The section settings in this example can be placed inside the sections' existing schema array, if it is already present. For example, it can be placed within the section setting named `Blog pages`. In this case, the code within the `{% schema %}` tags below can be added within the existing schema settings (including a comma to separate from other settings).
3.  By default, tags for all blogs will appear. This can be disabled on the [theme editor](https://help.shopify.com/en/themes/development/theme-editor).

```liquid
<aside>
  {%- if section.settings.blog_show_tags -%}
    <ul>
      <li>
        {%- for tag in blog.all_tags -%}
          <a href="{{ blog.url }}/tagged/{{ tag | handle }}">{{ tag }}</a>{% unless forloop.last %}, {% endunless %}
        {%- endfor -%}
      </li>
    </ul>
  {%- endif -%}
</aside>

{% schema %}
{
  "name": "Blog posts",
  "settings": [
    {
      "type": "checkbox",
      "id": "blog_show_tags",
      "label": "Show tags",
      "default": true
    }
  ]
}
{% endschema %}
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=tags&type=Code}">`#tags`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= sidebar&type=Code}">`#sidebar`</a> 

------
### <a name="comment-list">Comment list</a>
One of the biggest benefits of hosting a blog on an online store is that the audience can interact with merchants by commenting on blog posts. This code component will display the comments for a specific blog post.

1.  The following code should be added to the file that contains code for article content, such as `article.liquid`.
2.  Code to display article content should be placed below variable tags and above `{% if blog.comments_enabled? %}`.

```liquid
{%- assign new_comment = false -%}
{% if comment and comment.created_at %}
  {%- assign new_comment = true -%}
  {%- assign new_comment_id = comment.id -%}
{% endif %}

{% if new_comment %}
  {%- assign duplicate_comment = false %}
  {% for comment in article.comments %}
    {% if comment.id == new_comment_id %}
      {%- assign duplicate_comment = true %}
      {% break %}
    {% endif %}
  {% endfor %}

  {% if duplicate_comment %}
    {%- assign number_of_comments = article.comments_count -%}
  {% else %}
    {%- assign number_of_comments = article.comments_count | plus: 1 -%}
  {% endif %}
  {% else %}
    {%- assign number_of_comments = article.comments_count -%}
{% endif %}

<!-- Code to output article content can be added here. -->

{% if blog.comments_enabled? %}
  {% if number_of_comments > 0 %}
    <hr aria-hidden="true">
    <h2>{{ article.comments_count }}</h2>
    <ul>

      <!-- If a comment was just submitted with no blank field, show it. -->
      {% if new_comment %}
        <li>
          {{ comment.content }}
          <span>{{ comment.author }}</span>
  	      <span>{{ comment.created_at | time_tag: format: 'month_day_year' }}</span>
        </li>
      {% endif %}

      {% for comment in article.comments %}
        {% unless comment.id == new_comment_id %}
          <li>
            {{ comment.content }}
            <span>{{ comment.author }}</span>
  	        <span>{{ comment.created_at | time_tag: format: 'month_day_year' }}</span>
          </li>
        {% endunless %}
      {% endfor %}

    </ul>
  {% endif %}
{% endif %}
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=variable-tags&type=Code}">`#variable-tags`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= article&type=Code}">`#article`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= comments&type=Code}">`#comments`</a> 

------
### <a name="featured-blog-posts">Featured blog posts</a>
Merchants will often want to display previews of recent blog articles on their homepage. This featured blog posts section allows merchants to choose how many articles appear in the section, as well as turn on and off the visibility of the author info and publish date.

1.  Create a new file within the `sections` folder of your theme, and paste the code below into this file. Save file as `featured-blog.liquid`.
2.  Navigate to the [theme editor](https://help.shopify.com/en/themes/development/theme-editor) and select **"Add section"**. Within the blog category there will be an option for **"Blog posts"**.
3.  When added to the homepage, merchants can choose the number of blog posts they wish to appear, as well as make the author info and publish date visible or not visible.

```liquid
<h1>{{ section.settings.title | escape }}</h1>

{%- assign blog = blogs[section.settings.blog] -%}

{%- if blog.articles_count > 0 -%}
  <ul>

    {%- for article in blog.articles limit: section.settings.post_limit -%}
      <li>
        <a href="{{ article.url }}">
          {%- if article.image -%}
            {{ article | img_url: '150x150', scale: 2 | img_tag: '' }}
          {%- endif -%}
          <h2>{{ article.title }}</h2>
        </a>

        {%- if section.settings.blog_show_author -%}
          <span>
            By {{ article.author }}
          </span>
        {%- endif -%}

        {%- if section.settings.blog_show_date -%}
          <span>
            {{ article.published_at | time_tag: format: 'month_day_year' }}
          </span>
        {%- endif -%}

        {%- if article.excerpt.size > 0 -%}
          {{ article.excerpt }}
        {%- else -%}
          {{ article.content | strip_html | truncate: 150 }}
        {%- endif -%}

        {%- if article.tags.size > 0 -%}
          <ul aria-label="{{ 'blogs.article.tags' }}">
          {%- for tag in article.tags -%}
            <li>
              <a href="{{ blog.url }}/tagged/{{ tag | handle }}">{{ tag }}</a>
            </li>
          {%- endfor -%}
          </ul>
        {%- endif -%}

        <ul>
          <li>
            <a href="{{ article.url }}" aria-label="Read more: {{ article.title }}">
              Read more
            </a>
          </li>

          {%- if blog.comments_enabled? and article.comments_count > 0 -%}
            <li>
              <a href="{{ article.url }}#comments">
                {{ article.comments_count }} comments
              </a>
            </li>
          {%- endif -%}

        </ul>
      </li>
    {%- endfor -%}

  </ul>
{%- endif -%}

{%- if section.settings.show_view_all -%}
  <a href="{{ blog.url }}"
    class="btn"
    aria-label="{{ 'blogs.article.view_all_blogs' }}">
    {{ 'blogs.article.view_all' }}
  </a>
{%- endif -%}

{% schema %}
{
  "name": "Blog posts",
  "class": "index-section",
  "settings": [
    {
      "type": "text",
      "id": "title",
      "label": "Heading",
      "default": "Blog posts"
    },
    {
      "id": "blog",
      "type": "blog",
      "label": "Blog"
    },
    {
      "type": "range",
      "id": "post_limit",
      "label": "Posts",
      "min": 3,
      "max": 12,
      "step": 3,
      "default": 3
    },
    {
      "type": "checkbox",
      "id": "blog_show_author",
      "label": "Show author",
      "default": false
    },
    {
      "type": "checkbox",
      "id": "blog_show_date",
      "label": "Show date",
      "default": true
    },
    {
      "type": "checkbox",
      "id": "show_view_all",
      "label": "Show 'View all' button",
      "default": false
    }
  ],
  "presets": [
    {
      "name": "Blog posts",
      "category": "Blog",
      "settings": {
        "blog": "News",
        "post_limit": 3
      }
    }
  ]
}
{% endschema %}
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=homepage&type=Code}">`#homepage`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= images&type=Code}">`#images`</a> 

------
### <a name="previous-and-next-article-buttons">Previous and next article buttons</a>
Merchants may want to add an extra layer of navigation to blog posts. Previous and next article buttons allow for an easy way to navigate to additional articles.

1.  Locate the file that contains your theme's blog article listing, and paste the code below into this file. Code can be placed in the most appropriate position (such as above the social sharing buttons).
2.  Ensure code within `schema` tags is within the existing `schema` tags for the section.
3.  By default, buttons for all articles will appear. Buttons can be enabled or disabled from the [theme editor](https://help.shopify.com/en/themes/development/theme-editor).

```liquid
{% if section.settings.blog_show_previous_and_next_buttons %}
  <ul>
    <li>
      <a href="{{ blog.previous_article }}">Previous post</a>
    </li>
    <li>
      <a href="{{ blog.next_article }}">Next post</a>
    </li>
  </ul>
{%- endif -%}

{% schema %}
{
  "name": "Blog posts",
  "settings": [
    {
      "type": "checkbox",
      "id": "blog_show_previous_and_next_buttons",
      "label": "Show next/ previous buttons",
      "default": true
    }
  ]
}
{% endschema %}
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=article&type=Code}">`#article`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= pagination&type=Code}">`#pagination`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= buttons&type=Code}">`#buttons`</a> 
