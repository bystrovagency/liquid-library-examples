# Cart 

 ## Table of contents
1. [Cart notes](#cart-notes) 
2. [Checkout form](#checkout-form) 
 
 
------
### <a name="cart-notes">Cart notes</a>
You can add a text area to the cart page that allows customers to share special instructions for their order. Cart notes are submitted with a customer's order, and will appear on their order page in the Shopify admin.

1.  Add the following code to the `cart.liquid` file, inside the form. Cart notes typically appear just before the checkout button. Make sure that you keep `name="note"` so that cart notes are submitted correctly.

```liquid
<label for="CartNote">Special instructions</label>
<textarea name="note" id="CartNote">{{ cart.note }}</textarea>
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=cart-notes&type=Code}">`#cart-notes`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= special-instructions&type=Code}">`#special-instructions`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= instructions&type=Code}">`#instructions`</a> 

------
### <a name="checkout-form">Checkout form</a>
The checkout form is what customers use to review their cart, remove any unwanted products, and proceed to checkout.

1.  Place the following code in the cart.liquid template file. If this file doesn't exist, create a new file in the `template` folder and name it `cart.liquid`.

```liquid
{%- if cart.item_count > 0 -%}

<form action="/cart" method="post">

  {%- for item in cart.items -%}
    <a href="{{ item.url | within: collections.all }}">
      <img src="{{ item | img_url: '200x200' }}" alt="{{ item.image.alt | escape }}">
      {{ item.product.title }}
    </a>

    {%- unless item.variant.title contains 'Default' -%}
      <p>{{ item.variant.title }}</p>
    {%- endunless -%}

    {%- assign property_size = item.properties | size -%}
    {%- if property_size > 0 -%}
      <ul>

        {%- for p in item.properties -%}
          {%- assign first_character_in_key = p.first | truncate: 1, '' -%}
          {%- unless p.last == blank or first_character_in_key == '_' -%}
            <li>
              {{ p.first }}:

              {%- if p.last contains '/uploads/' -%}
                <a href="{{ p.last }}">{{ p.last | split: '/' | last }}</a>
              {%- else -%}
                {{ p.last }}
              {%- endif -%}

            </li>
          {%- endunless -%}
        {%- endfor -%}

      </ul>
    {%- endif -%}

    <p>
      <a aria-label="Remove {{ item.variant.title }}" href="/cart/change?line={{ forloop.index }}&amp;quantity=0">Remove</a>
    </p>
  {%- endfor -%}

  <input type="submit" name="checkout" value="Checkout">
</form>

{%- else -%}
  <p>The cart is empty. <a href="/collections/all">Continue shopping</a></p>
{%- endif -%}
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=cart&type=Code}">`#cart`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= checkout&type=Code}">`#checkout`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= form&type=Code}">`#form`</a> 
