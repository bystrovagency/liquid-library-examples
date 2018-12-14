# Products

Products are the goods, digital downloads, services, and gift cards that you sell in an online store. Products can contain a lot of information, and can be rendered and sorted in a variety of ways. These examples showcase ways of rendering product data and listing products. This includes how to render metafields, SKUs, variant options, recommended products, and more. 

 ## Table of contents
1. [Product metafields](#product-metafields) 
2. [Variant images](#variant-images) 
3. [Product variant selector](#product-variant-selector) 
4. [Recommended products by collection](#recommended-products-by-collection) 
5. [Recommended products by tag](#recommended-products-by-tag) 
6. [Show product SKU](#show-product-sku) 
7. [Single variant product](#single-variant-product) 
 
 
------
### <a name="product-metafields">Product metafields</a>
Product metafields store and display additional product information that doesn't otherwise exist in the Shopify Admin. This example demonstrates how to add washing instructions to your products and display them on the product page. See more examples of how to use product metafields in the [Shopify Help Center](https://help.shopify.com/en/themes/liquid/objects/metafield).

1.  Install a [Metafields app from the Shopfiy App Store](https://apps.shopify.com/search?q=metafield).
2.  Create a new metafield. For the namespace, enter `instructions`, for the key enter `wash`, and for the value enter `Cold water`.
3.  Create another metafield. For the namespace enter `instructions`, for the key enter `dry` and for the value enter `Tumble dry`.
4.  Add the following code to either your `product.liquid` template file, or a `product-template.liquid` section file.

```liquid
{%- if product.metafields.instructions != blank -%}
  <ul>
    {%- for field in product.metafields.instructions -%}
      <li>{{ field | first }}: {{ field | last }}</li>
    {%- endfor -%}
  </ul>
{%- endif -%}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=metafields&type=Code}">`#metafields`</a> 

------
### <a name="variant-images">Variant images</a>
[Deep-linking](https://www.shopify.com/partners/blog/product-variants) can be used to pre-select a specific variant on a product page. In order to load the image of a deep-linked product, the `product.selected_or_first_available_variant.featured_image` Liquid attribute must be referenced. This code example shows how this object can be used on a product page. This example demonstrates how this object can be used on a product page.

1.  Add the code below where you would like to output a product's featured image on a product page.
2.  The default filter is used to select the product's `featured_image` if a variant image does not exist.

```liquid
{%- assign featured_image = product.selected_or_first_available_variant.featured_image | default: product.featured_image -%}

<a href="{{ featured_image | img_url: '1024x' }}">
  <img src="{{ featured_image | img_url: '1024x' }}" alt="{{ featured_image.alt | escape }}">
</a>
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=product-variant&type=Code}">`#product-variant`</a> 

------
### <a name="product-variant-selector">Product variant selector</a>
The product variant selector is the HTML control which a user would interact with in order to select a product variant. Depending on the theme settings, the controls could be radio buttons or a select drop-down.

1.  Place the following code in the `product-template.liquid` file, withing the `{% form 'product' … %}` block. If this file doesn't exist, then click **Add a new section** and call it `product-template`.
2.  Make sure that the `product.liquid` file includes the following Liquid tag: `{% section 'product-template' %}`. Add this tag to the file if it isn't there already.

```liquid
{%- unless product.has_only_default_variant -%}
  {%- for option in product.options_with_values -%}

    {%- if section.settings.product_selector == 'radio' -%}
      <fieldset id="ProductSelect-option-{{ forloop.index0 }}" name="{{ option.name | handleize }}">
        <legend>
          {{ option.name | escape }}
        </legend>
        {%- for value in option.values -%}
          <!-- Check to see if there's a product size option. If there is a size, check to see if there's any availble for purchase. If not, set the variat control in a "disabled" state. -->
          {%- assign variant_label_state = true -%}

          {%- if product.options.size == 1 -%}
            {%- unless product.variants[forloop.index0].available -%}
              {%- assign variant_label_state = false -%}
            {%- endunless -%}
          {%- endif -%}

          <input type="radio"
            {% if option.selected_value == value %} checked="checked"{% endif %}
            {% unless variant_label_state %} disabled="disabled"{% endunless %}
            value="{{ value | escape }}"
            data-index="option{{ forloop.index }}"
            name="{{ option.name | handleize }}"
            id="ProductSelect-option-{{ option.name | handleize }}-{{ value | escape }}">
          <label for="ProductSelect-option-{{ option.name | handleize }}-{{ value | escape }}">
            {{ value | escape }}
          </label>
        {%- endfor -%}
      </fieldset>
    {%- else -%}
      <label for="ProductSelect-option-{{ forloop.index0 }}">
        {{ option.name | escape }}
      </label>
      <select id="ProductSelect-{{ forloop.index0 }}" data-index="option{{ forloop.index }}">
        {%- for value in option.values -%}
          <option value="{{ value | escape }}"{% if option.selected_value == value %} selected="selected"{% endif %}>
            {{ value | escape }}
          </option>
        {%- endfor -%}
      </select>
    {%- endif -%}

  {%- endfor -%}
{%- endunless -%}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=product-variant&type=Code}">`#product-variant`</a> 

------
### <a name="recommended-products-by-collection">Recommended products by collection</a>
A recommended products section helps to drive conversions by making it easy for customers to continue shopping. Recommended products are often displayed at the bottom of the product page. This example features products within the same collection.

1.  Add a new section in the `sections` directory and name it `recommended-products`. Paste the code below in the new file.
2.  Locate the `product.liquid` template and add the following Liquid tag where you would like the recommended products to appear: `{% section 'recommended-products' %}`

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

{%- assign number_of_products = section.settings.number_of_products -%}
{%- assign number_of_products_to_fetch = number_of_products | plus: 1 -%}

{%- if collection == null or collection.handle == 'frontpage' or collection.handle == 'all' -%}
  {%- assign found_a_collection = false -%}
  {%- for c in product.collections -%}

    {%- if found_a_collection == false and c.handle != 'frontpage' and c.handle != 'all' and c.all_products_count > 1 -%}
      {%- assign found_a_collection = true -%}
      {%- assign collection = c -%}
    {%- endif -%}

  {%- endfor -%}
{%- endif -%}

{%- if collection and collection.products_count > 1 -%}
  <h2>{{ section.settings.heading }}</h2>

  <ul>
    {%- assign current_product = product -%}
    {%- assign current_product_found = false -%}
    {%- for product in collection.products limit: number_of_products_to_fetch -%}

      {%- if product.handle == current_product.handle -%}
        {%- assign current_product_found = true -%}
      {%- else -%}
        {%- unless current_product_found == false and forloop.last -%}
          <li>
            <a href="{{ product.url | within: collection }}" class="product-card">
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
        {%- endunless -%}
      {%- endif -%}

    {%- endfor -%}
  </ul>
{%- endif -%}

{% schema %}
{
  "name": "Recommended products",
  "settings": [
    {
      "type": "text",
      "id": "heading",
      "label": "Heading",
      "default": "Recommended products"
    },
    {
      "type": "select",
      "id": "number_of_products",
      "label": "Number of products to show",
      "default": "4",
      "options": [
        {
          "value": "4",
          "label": "4"
        },
        {
          "value": "8",
          "label": "8"
        },
        {
          "value": "12",
          "label": "12"
        }
      ]
    }
  ]
}
{% endschema %}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=recommended-products&type=Code}">`#recommended-products`</a> 

------
### <a name="recommended-products-by-tag">Recommended products by tag</a>
A recommended products section helps to drive conversions by making it easy for customers to continue shopping. Recommended products are often displayed at the bottom of the product page. This example features products with the same tag.

1.  Add a new section in the `sections` directory and name it `recommended-products`. Paste the code below in the new file.
2.  Locate the `product.liquid` template and add the following Liquid tag where you would like the recommended products to appear: `{% section 'recommended-products' %}`

```liquid
<style>
  .product-card {
    box-sizing: border-box;
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

{%- assign counter = 0 -%}
{%- assign break_at = section.settings.number_of_products | plus: 0 -%}
{%- assign current_product = product -%}

{%- capture related_items -%}
  {%- for product in collections.all.products -%}
    {%- unless product.handle == current_product.handle -%}

      {%- if product.tags contains section.settings.related_tag -%}
        <a href="{{ product.url | within: collection }}" class="product-card">
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

        {%- assign counter = counter | plus: 1 -%}

        {%- if counter == break_at -%}
          {%- break -%}
        {%- endif -%}

      {%- endif -%}

    {%- endunless -%}
  {%- endfor -%}
{%- endcapture -%}

{%- assign related_items = related_items | trim -%}
{%- unless related_items == blank -%}
  <aside>
    {%- if section.settings.heading -%}
      <h2>{{ section.settings.heading }}</h2>
    {%- endif -%}

    {{ related_items }}
  </aside>
{%- endunless -%}

{% schema %}
{
  "name": "Recommended products",
  "settings": [
    {
      "type": "text",
      "id": "heading",
      "label": "Heading",
      "default": "Recommended products"
    },
    {
      "type": "text",
      "id": "related_tag",
      "label": "Tag",
      "info" : "The tag determines which products show as related products."
    },
    {
      "type": "select",
      "id": "number_of_products",
      "label": "Number of products to show",
      "default": "4",
      "options": [
        {
          "value": "4",
          "label": "4"
        },
        {
          "value": "8",
          "label": "8"
        },
        {
          "value": "12",
          "label": "12"
        }
      ]
    }
  ]
}
{% endschema %}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=recommended-products&type=Code}">`#recommended-products`</a> 

------
### <a name="show-product-sku">Show product SKU</a>
Stock keeping units (SKUs) are used to identify products and track inventory. You can display the unique SKU for a product and its variants on the product page.

1.  Add the following code to the `product-template.liquid` section, just below the product title.
2.  Depending on your theme, ensure that the SKU updates dynamically when you select other variants. If not, then you will need to add [a few lines of JavaScript](https://help.shopify.com/en/themes/customization/products/features/show-sku-numbers#show-sku-numbers-on-product-pages-sectioned-themes-specific) to your theme's JavaScript file.

```liquid
{%- assign current_variant = product.selected_or_first_available_variant -%}
<span>{{ current_variant.sku }}</span>
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=variants&type=Code}">`#variants`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= sku&type=Code}">`#sku`</a> 

------
### <a name="single-variant-product">Single variant product</a>
The product page is a detailed page for an individual product. It includes information such as the product title, description, price, vendor, variants, and images, along with a dynamic checkout button, and an add to cart button. This product page is for single variant products.

1.  Place the following code in the `product-template.liquid` file. If this file doesn't exist, then create a new file in the `sections` folder and name it `product-template`.
2.  Ensure that the `product.liquid` file includes the following Liquid tag: `{% section 'product-template' %}`.

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

<h2>{{ product.title }}</h2>

<dl>
  <dt><span class="visuallyhidden">Regular price</span></dt>
  <dd>{{ product.price | money }}</dd>

  {%- if product.vendor -%}
    <dt><span class="visuallyhidden">Vendor</span></dt>
    <dd>{{ product.vendor }}</dd>
  {%- endif -%}

</dl>

<p>{{ product.description }}</p>

{%- assign featured_image = product.featured_image -%}
<img src="{{ featured_image | img_url: '1024x'}}" alt="{{ featured_image.alt | escape }}">

{% form 'product', product %}
<select name="id">
  {%- for variant in product.variants -%}
    <option selected="selected" value="{{ variant.id }}"></option>
  {%- endfor -%}
</select>

<button type="submit" {% unless product.available %} disabled="disabled"{% endunless %}>
  {%- if product.available -%}
    Add to Cart
  {%- else -%}
    Sold Out
  {%- endif -%}
</button>
{{ form | payment_button }}
{% endform %}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=images&type=Code}">`#images`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= page&type=Code}">`#page`</a> 
