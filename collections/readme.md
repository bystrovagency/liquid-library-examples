# Collections

Products can be organized into collections to make it easier for customers to find them by a particular category. These examples demonstrate how to render a collection list, list products in a collection, set how many products are rendered per page, and display a list of vendors. 

 ## Table of contents
1. [Collection list](#collection-list) 
2. [Collection page](#collection-page) 
3. [Product limit](#product-limit) 
4. [Vendor link list](#vendor-link-list) 
 
 
------
### <a name="collection-list">Collection list</a>
A collection list is a page that displays all the collections in a store. In this example, a featured image for the collection is displayed, as well as the collection name.

1.  Place the following code in the `list-collections.liquid` file. If this file doesn't exist, create one in the `Templates` directory of your theme.

```liquid
<h1>{{ page_title }}</h1>

<ul>
  {%- for collection in collections -%}
    <li>
      <!--
        These control flow tags check to see if there is a featured image for a collection.
        If there isn't one, then we assign the image from the first product in the collection.
      -->
      {%- if collection.image -%}
        {%- assign collection_image = collection.image -%}
      {%- elsif collection.products.first and collection.products.first.images != empty -%}
        {%- assign collection_image = collection.products.first.featured_image -%}
      {%- else -%}
        {%- assign collection_image = blank -%}
      {%- endif -%}

      <a href="{{ collection.url }}">
        <img src="{{ collection_image | img_url: '480x' }}" alt="">
        {{ collection.title }}
      </a>
    </li>
  {%- endfor -%}
</ul>
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=collection-list&type=Code}">`#collection-list`</a> 

------
### <a name="collection-page">Collection page</a>
The collection page lists the products within a collection. There is a limit of 50 products per page, after which pagination will automatically occur. Learn more about the collection template file in the [Shopify Help Center](https://help.shopify.com/en/themes/development/templates/collection-liquid).

1.  Place the following code in the `collection.liquid` file. If this file doesn't exist, add it to the `Templates` folder.

```liquid
<style>
  .product-card {
    box-sizing: border-box;
    float: left;
    min-height: 1em;
    padding-left: 2em;
    vertical-align: top;
    width: 25%;
  }

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

<h1>{{ collection.title }}</h1>

{%- if collection.description != blank -%}
  <p>{{ collection.description }}</p>
{%- endif -%}

<ul>
  {%- for product in collection.products -%}
    <li>
      <a class="product-card" href="{{ product.url | within: collection }}">
        <img src="{{ product.featured_image.src | img_url: '1024x' }}" alt="">
        {{ product.title }}
        <p>
          <span aria-hidden="true">—</span>
          {%- if product.price_varies -%}
            <span class="visuallyhidden">Starting at</span>
            {{ product.price_min | money_without_trailing_zeros }}
            <span aria-hidden="true">+</span>
          {%- else -%}
            {{ product.price | money_without_trailing_zeros }}
          {%- endif -%}
        </p>
        <p>
          <span class="visuallyhidden">by</span>
          {{ product.vendor }}
        </p>
      </a>
    </li>
  {%- endfor -%}
</ul>
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=page&type=Code}">`#page`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= product-list&type=Code}">`#product-list`</a> 

------
### <a name="product-limit">Product limit</a>
A product limit sets a limit on the number of products rendered on a single page. This example demonstrates how to limit the number of products that show per collection page. Learn more about product limits in the [Shopify Help Center](https://help.shopify.com/en/themes/customization/collections/show-more-products-on-collection-pages).

1.  Place the paginate tags around a `forloop`, that loops through products within a collection.
2.  Replace `limit` with the number of products you wish to show per page on the collection.

```liquid
{% paginate collection.products by limit %}
  <-- Product 'forloop' content goes here -->
{% endpaginate %}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=product-limit&type=Code}">`#product-limit`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= pagination&type=Code}">`#pagination`</a> 

------
### <a name="vendor-link-list">Vendor link list</a>
A vendor is usually the manufacturer, wholesaler, or creator of a product. This example creates a list of all the vendors for a store. Each vendor name links to a collection page that is filtered to show products by that particular vendor.

1.  Place the following code where you would like the link list to appear, such as a content page, on a blog sidebar, or on a collection page.

```liquid
<ul>
  {%- for product_vendor in shop.vendors -%}
    <li>{{ product_vendor | link_to_vendor }}</li>
  {%- endfor -%}
</ul>
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=collections&type=Code}">`#collections`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= vendor-page&type=Code}">`#vendor-page`</a> 
