# Navigation

After you add products and create collections, pages, or blog posts, you need to organize them on your online store in a way that allows customers to find them. These examples showcase different types of navigational elements on Shopify, including nested navigation, breadcrumbs, pagination, and more.

 ## Table of contents
1. [Breadcrumb navigation](#breadcrumb-navigation)
2. [Nested navigation](#nested-navigation)
3. [Simple pagination](#simple-pagination)
4. [Pagination](#pagination)
5. [Social media navigation](#social-media-navigation)


------
### <a name="breadcrumb-navigation">Breadcrumb navigation</a>
A Breadcrumb navigation, also known as breadcrumbs, can reduce the number of actions a visitor needs to take in order to navigate to a higher-level page, and improve the discoverability of a website’s sections and pages. Like all Shopify based navigations, it uses the [linklist](https://help.shopify.com/en/themes/liquid/objects/linklist) object.

1.  Place the following code in the `theme.liquid` file, just inside the main content wrapper, or wherever you wish the breadcrumb to appear.
2.  To see the breadcrumbs appear, ensure that your site has already been populated with products, collections, blog posts and pages.

```liquid
<style>
  .breadcrumbs {
    margin: 0 0 2em;
  }

  .breadcrumbs__list {
    list-style-type: none;
    margin: 0;
    padding: 0;
  }

  .breadcrumbs__item {
    display: inline-block;
  }

  .breadcrumbs__item:not(:last-child):after {
    border-style: solid;
    border-width: .10em .10em 0 0;
    content: '';
    display: inline-block;
    height: .20em;
    margin: 0 .20em;
    position: relative;
    transform: rotate(45deg);
    vertical-align: middle;
    width: .20em;
  }

  .breadcrumbs__link {
    text-decoration: underline;
  }

  .breadcrumbs__link[aria-current="page"] {
    color: inherit;
    font-weight: normal;
    text-decoration: none;
  }

  .breadcrumbs__link[aria-current="page"]:hover,
  .breadcrumbs__link[aria-current="page"]:focus {
    text-decoration: underline;
  }
</style>

{%- unless template == 'index' or template == 'cart' or template == 'list-collections' or template == '404' -%}
{%- assign t = template | split: '.' | first -%}

<nav class="breadcrumbs" role="navigation" aria-label="breadcrumbs">
  <ol class="breadcrumbs__list">
    <li class="breadcrumbs__item">
      <a class="breadcrumbs__link" href="/">Home</a>
    </li>
    {%- case t -%}
      {%- when 'page' -%}
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link" href="{{ page.url }}" aria-current="page">{{ page.title }}</a>
        </li>
      {%- when 'product' -%}
        {%- if collection.url -%}
          <li class="breadcrumbs__item">
            {{ collection.title | link_to: collection.url }}
          </li>
        {%- endif -%}
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link" href="{{ product.url }}" aria-current="page">{{ product.title }}</a>
        </li>
      {%- when 'collection' and collection.handle -%}
        {%- if current_tags -%}
          <li class="breadcrumbs__item">
            {{ collection.title | link_to: collection.url }}
          </li>
          <li class="breadcrumbs__item">
            {%- capture tag_url -%}{{ collection.url }}/{{ current_tags | join: "+"}}{%- endcapture -%}
            <a class="breadcrumbs__link" href="{{ tag_url }}" aria-current="page">{{ current_tags | join: " + "}}</a>
          </li>
        {%- else -%}
          <li class="breadcrumbs__item">
            <a class="breadcrumbs__link" href="{{ collection.url }}" aria-current="page">{{ collection.title }}</a>
          </li>
        {%- endif -%}
      {%- when 'blog' -%}
        {%- if current_tags -%}
          <li class="breadcrumbs__item">
            {{ blog.title | link_to: blog.url }}
          </li>
          <li class="breadcrumbs__item">
            {%- capture tag_url -%}{{blog.url}}/tagged/{{ current_tags | join: "+" }}{%- endcapture -%}
            <a class="breadcrumbs__link" href="{{ tag_url }}" aria-current="page">{{ current_tags | join: " + " }}</a>
          </li>
        {%- else -%}
          <li class="breadcrumbs__item">
            <a class="breadcrumbs__link" href="{{ blog.url }}" aria-current="page">{{ blog.title }}</a>
          </li>
        {%- endif -%}
      {%- when 'article' -%}
        <li class="breadcrumbs__item">
          {{ blog.title | link_to: blog.url }}
        </li>
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link" href="{{ article.url }}" aria-current="page">{{ article.title }}</a>
        </li>
      {%- else -%}
        <li class="breadcrumbs__item">
          <a class="breadcrumbs__link" href="{{ request.path }}" aria-current="page">{{ page_title }}</a>
        </li>
    {%- endcase -%}
  </ol>
</nav>
{%- endunless -%}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=linklist&type=Code}">`#linklist`</a>

------
### <a name="nested-navigation">Nested navigation</a>
A nested navigation or nested menu is a popular solution for effectively organizing collections, products, and pages. This example will output a nested menu of links in an unordered list up to three levels deep, all of which can be easily updated using the Shopify Admin. A nested navigation uses the Shopify [linklist](https://help.shopify.com/en/themes/liquid/objects/linklist) object.

1.  Place the following code in the `Sections/header.liquid` file, or wherever you wish the nested navigation to appear.
2.  If used in a section, you can also create a settings option in the section schema, and replace the `forloop` in this example with `{% for link in linklists[section.settings.main_linklist].links %}` where `main_linklist` is the `id` of the schema setting. This setting would specify a `link_list` picker, and the `default` value would be set to `main-menu`.
3.  Add relevant classes and styles to create dropdown functionality via CSS.

```liquid
<nav role="navigation">
  <ul>
    {%- for link in linklists.main-menu.links -%}
      <li>
        <a href="{{ link.url }}" {% if link.active %}aria-current="page"{% endif %}>
          {{ link.title }}
        </a>

        {%- if link.links != blank -%}
          <ul>
            {%- for child_link in link.links -%}
              <li>
                <a href="{{ child_link.url }}" {% if child_link.active %}aria-current="page"{% endif %}>
                  {{ child_link.title }}
                </a>

                {%- if child_link.links != blank -%}
                  <ul>
                    {%- for grandchild_link in child_link.links -%}
                      <li>
                        <a href="{{ grandchild_link.url }}" {% if grandchild_link.active %}aria-current="page"{% endif %}>
                          {{ grandchild_link.title }}
                        </a>
                      </li>
                    {%- endfor -%}
                  </ul>
                {%- endif -%}

              </li>
            {%- endfor -%}
          </ul>
        {%- endif -%}

      </li>
    {%- endfor -%}
  </ul>
</nav>
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=dropdown-menu&type=Code}">`#dropdown-menu`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= nested&type=Code}">`#nested`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= linklist&type=Code}">`#linklist`</a>

------
### <a name="simple-pagination">Simple pagination</a>
Pagination is an ordered numbering of pages usually located at the top or bottom of a webpage. It enables users to navigate through a series of pages where content has been split up for design purposes, usability, faster loading, an so on. Note: this simplified example does not output a fully accessible pagination. For an accessible pagination, see the next Pagination example.

1.  Place the following code where you wish pagination to display. This code must appear within `paginate` tags for the following example to work. Within the `paginate` tags, you can access the [paginate](https://help.shopify.com/en/themes/liquid/objects/paginate) object.
2.  Specify the type of content you want to paginate, and at what limit you wish to paginate by, for example `{% paginate collection.products by 12 %}`.
3.  Override the`| default_pagination` filter `Next »` and `« Previous` labels by passing a new label to the `next` and `previous` options.

```liquid
{%- paginate blog.articles by 5 -%}
  {%- for product in collection.products -%}
    <!-- show product details here -->
  {%- endfor -%}

  {{ paginate | default_pagination: next: 'Older', previous: 'Newer' }}
{%- endpaginate -%}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=collection&type=Code}">`#collection`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= products&type=Code}">`#products`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= pagination&type=Code}">`#pagination`</a>

------
### <a name="pagination">Pagination</a>
Pagination is an ordered numbering of pages usually located at the top or bottom of a webpage. It enables users to navigate through a series of pages where content has been split up for design purposes, usability, faster loading, and so on. Splitting products, blog articles, and search results across multiple pages is a necessary part of theme design as you are limited to 50 results per page in any `forloop`.

1.  Place the following code where you wish pagination to display. This code must appear within `paginate` tags for the following example to work. Within the `paginate` tags, you can access the [paginate](https://help.shopify.com/en/themes/liquid/objects/paginate) object.
2.  Specify the type of content you want to paginate, and at what limit you wish to paginate by, for example `{% paginate collection.products by limit %}` or `{% paginate blog.articles by 12 %}`.

```liquid
<style>
.visuallyhidden {
  border: 0;
  clip: rect(0 0 0 0);
  height: 1px;
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
  width: 1px;
  white-space: nowrap;
}
</style>

{%- paginate blog.articles by 5 -%}
  {%- for article in blog.articles -%}
    <!-- show blog article details here -->
  {%- endfor -%}

  {%- if paginate.pages > 1 -%}
    <nav role="navigation">
      <ol class="pagination">
        {%- if paginate.previous-%}
          <li>
            <a href="{{ paginate.previous.url }}">
              <span aria-hidden="true">&laquo;</span>
              Previous <span class="visuallyhidden">page</span>
            </a>
          </li>
        {%- else -%}
          <li class="disabled">
            <span aria-hidden="true">&laquo;</span>
            Previous <span class="visuallyhidden">page</span>
          </li>
        {%- endif -%}

        {%- for part in paginate.parts -%}
          {%- if part.is_link -%}
            <li>
              <a href="{{ part.url }}">
                <span class="visuallyhidden">page</span> {{ part.title }}
              </a>
            </li>
          {%- else -%}
            {%- if part.title == paginate.current_page -%}
              <li class="active" aria-current="page">
                <span class="visuallyhidden">page</span> {{ part.title }}
              </li>
            {%- else -%}
              <li>
                <span class="visuallyhidden">page</span> {{ part.title }}
              </li>
            {%- endif -%}
          {%- endif -%}
        {%- endfor -%}

        {%- if paginate.next -%}
          <li>
            <a href="{{ paginate.next.url }}">
              Next <span class="visuallyhidden">page</span>
              <span aria-hidden="true">&raquo;</span>
            </a>
          </li>
        {%- else -%}
          <li class="disabled">
            Next <span class="visuallyhidden">page</span>
            <span aria-hidden="true">&raquo;</span>
          </li>
        {%- endif -%}
      </ol>
    </nav>
  {%- endif -%}
{%- endpaginate -%}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=article&type=Code}">`#article`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= blog&type=Code}">`#blog`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= pagination&type=Code}">`#pagination`</a>

------
### <a name="social-media-navigation">Social media navigation</a>
Social media navigation is a static menu linking to various social media accounts using icons. Please note that this example uses an iconic font to display the social media icons. We highly recommend that you use inline SVG icons, or a third party service to generate an SVG sprite map of icons to use via class name such as [Icomoon](https://icomoon.io/) or [Fontastic](http://app.fontastic.me)

1.  Place the following code where you would like your social media navigation to appear.
2.  The `visually-hidden` class hides the link text from the screen so only the icon is visible, and the `aria-describedby` attribute lets people using screen readers know that the link will open a new window. A longer explaination can be found in [this article](https://medium.com/@svinkle/why-let-someone-know-when-a-link-opens-a-new-window-8699d20ed3b1).

```liquid
{{ 'social/social-icons.css' | global_asset_url | stylesheet_tag }}

<div hidden>
  <span id="new-window-0">Opens in a new window</span>
</div>

<ul class="social-media-menu">
  <li>
    <a href="https://facebook.com/shopify"
      target="_blank"
      rel="noopener"
      aria-label="Facebook"
      aria-describedby="new-window-0">
      <span class="shopify-social-icon-facebook-circle" aria-hidden="true"></span>
    </a>
  </li>
  <li>
    <a href="https://twitter.com/shopify"
      target="_blank"
      rel="noopener"
      aria-label="Twitter"
      aria-describedby="new-window-0">
      <span class="visuallyhidden">Twitter</span>
      <span class="shopify-social-icon-twitter-circle" aria-hidden="true"></span>
    </a>
  </li>
  <li>
    <a href="https://pinterest.com/shopify/"
      target="_blank"
      rel="noopener"
      aria-label="Pinterest"
      aria-describedby="new-window-0">
      <span class="shopify-social-icon-pinterest-circle" aria-hidden="true"></span>
    </a>
  </li>
  <li>
    <a href="https://instagram.com/shopify/"
      target="_blank"
      rel="noopener"
      aria-label="Instagram"
      aria-describedby="new-window-0">
      <span class="shopify-social-icon-instagram-circle" aria-hidden="true"></span>
    </a>
  </li>
</ul>
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=socialmedia&type=Code}">`#socialmedia`</a>
