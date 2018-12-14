# Cart

The cart page shows a summary of all of the products that a customer has added to the cart, a subtotal and a total price for the order, and a checkout button that directs customers to Shopify's secure checkout pages. These examples demonstrate how to add cart notes, and render the cart form. 

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
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=cart-notes&type=Code}">`#cart-notes`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= special-instructions&type=Code}">`#special-instructions`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= instructions&type=Code}">`#instructions`</a> 

------
### <a name="checkout-form">Checkout form</a>
The checkout form is what customers use to review their cart, remove any unwanted products, and proceed to checkout.

1.  Place the following code in the cart.liquid template file. If this file doesn't exist, create a new file in the `template` folder and name it `cart.liquid`.

```liquid
{%- comment -%}
  title: Checkout form
  description: The checkout form is what customers use to review their cart, to remove any unwanted products, and then to proceed to the checkout.
  tags: #cart, #checkout, #form
  last_updated: 2018-11-30
  step_1: Place the following code in the `cart.liquid` template file. If this file doesn't exist, then create a new file in the template folder and name it `cart.liquid`.
{%- endcomment -%}

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

{%- if cart.item_count > 0 -%}

<form action="/cart" method="post" novalidate>
  <table>
    <thead>
      <th colspan="2" scope="col">Product</th>
      <th scope="col">Price</th>
      <th scope="col">Quantity</th>
      <th scope="col">Total</th>
    </thead>
    <tbody>
      {%- for item in cart.items -%}
        <tr>
          <td>
            <img src="{{ item | img_url: '95x95', scale: 2 }}" alt="{{ item.title | escape }}" data-item-url="{{ item.url }}">
          </td>
          <td>
            <p><a href="{{ item.url }}">{{ item.product.title }}</a></p>

            {%- unless item.variant.title contains 'Default' -%}
              <ul>
                {%- for option in item.product.options -%}
                  <li>{{ option }}: {{ item.variant.options[forloop.index0] }}</li>
                {%- endfor -%}
              </ul>
            {%- endunless -%}

            {%- assign property_size = item.properties | size -%}
            {%- if property_size > 0 -%}
              <ul>
                {%- for p in item.properties -%}
                  {%- unless p.last == blank -%}
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

            <p><a href="/cart/change?line={{ forloop.index }}&amp;quantity=0" aria-label="Remove {{ item.product.title }}">Remove</a></p>
          </td>
          <td>
            {{ item.price | money }}
          </td>
          <td>
            <label for="updates_{{ item.key }}">Quantity</label>
            <input type="number" name="updates[]" id="updates_{{ item.key }}" value="{{ item.quantity }}" min="0" pattern="[0-9]*" data-quantity-item="{{ forloop.index }}">
          </td>
          <td>
            {%- if item.original_line_price != item.line_price -%}
              <span class="visuallyhidden">Original price</span>
              <s>{{ item.original_line_price | money }}</s>
              <span class="visuallyhidden">Sale price</span>
            {%- endif -%}
            {{ item.line_price | money }}

            {%- for discount in item.discounts -%}
             {{ discount.title }}
            {%- endfor -%}
          </td>
        </tr>
      {%- endfor -%}
    </tbody>
    <tfoot>
      <tr>
        <td colspan="4">
          <span>Subtotal</span>
        </td>
        <td>
          <span>{{ cart.total_price | money }}</span>
          {%- if cart.total_discounts > 0 -%}
            Savings
            <span>{{ cart.total_discounts | money }}</span>
          {%- endif -%}
        </td>
      </tr>
    </tfoot>
  </table>

  <span>Subtotal</span>
  <span>{{ cart.total_price | money }}</span>
  {%- if cart.total_discounts > 0 -%}
    Savings
    <span>{{ cart.total_discounts | money }}</span>
  {%- endif -%}

  {{ taxes_shipping_checkout }}
  <ul>
    <li><a href="collections/all">Continue shopping</a></li>
    <li><input type="submit" name="update" value="Update"></li>
    <li><input type="submit" name="checkout" value="Checkout"></li>
  </ul>

  {%- if additional_checkout_buttons -%}
    {{ content_for_additional_checkout_buttons }}
  {%- endif -%}
</form>

{%- else -%}
  <p>The cart is empty. <a href="/collections/all">Continue shopping</a></p>
{%- endif -%}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=cart&type=Code}">`#cart`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= checkout&type=Code}">`#checkout`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= form&type=Code}">`#form`</a> 
