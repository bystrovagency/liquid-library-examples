# Global

Some components of an online store exist on all pages, usually in the header and footer of a website. These are considered global components, and are often necessary and considered best practice. These examples showcase some page templates that render outside of the typical scope of products, pages, and collections, as well as header logo, social media metadata, customer links, accepted payments, and more. 

 ## Table of contents
1. [404 page](#404-page) 
2. [Copyright text](#copyright-text) 
3. [Customer account links](#customer-account-links) 
4. [Customer login](#customer-login) 
5. [Header logo](#header-logo) 
6. [OG tags](#og-tags) 
7. [Password page](#password-page) 
8. [Payment icons](#payment-icons) 
9. [Skip link](#skip-link) 
 
 
------
### <a name="404-page">404 page</a>
A 404 page appears when a website is active, but the specific page being requested within it doesn’t exist. This example demonstrates the markup required to output a “page not found” message, and includes a search field to help a customer find what they might be looking for.

1.  Place the following code in the `Templates/404.liquid` file.

```liquid
<section>
  <h2>Page not found</h2>
  <p>The page you requested is not here. If you feel like this is incorrect, you can drop us a <a href="/pages/help/">note</a>, or head back to the <a href="{{ shop.url }}">storefront</a>.</p>
  <form action="{{ shop.url }}/search" method="get" accept-charset="utf-8" role="search">
    <label for="q">Search</label>
    <input type="search" id="q" name="q" />
    <button>Search</button>
  </form>
</section>
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=404&type=Code}">`#404`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= page&type=Code}">`#page`</a> 

------
### <a name="copyright-text">Copyright text</a>
Copyright text is typically displayed in the footer section of an online store, and provides a clear indication of a copyright symbol, the year of creation and author of the content. This example includes the copyright symbol, the current year, your store name, and a "Powered by Shopify" link.

1.  Add the following code into your `footer.liquid` section, or wherever you wish the copyright to appear.

```liquid
<p>&copy; {{ 'now' | date: '%Y' }}, {{ shop.name | link_to: '/' }}</p>
<p>{{ powered_by_link }}</p>
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=copyright&type=Code}">`#copyright`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= footer&type=Code}">`#footer`</a> 

------
### <a name="customer-account-links">Customer account links</a>
Customer account links allow customers to log into their existing account or to create a new account on a Shopify store. These links typically appear in the header area of a website.

1.  Add the following code to the `header.liquid` section file, or in the place that you want the customer account links to appear.

```liquid
{%- if shop.customer_accounts_enabled -%}
  <ul>
    {%- if customer -%}
      <li>
        <a href="/account">Account</a>
      </li>
      <li>
        {{ 'Log out' | customer_logout_link }}
      </li>
    {%- else -%}
      <li>
        {{ 'Log in' | customer_login_link }}
      </li>
      <li>
        {{ 'Create account' | customer_register_link }}
      </li>
    {%- endif -%}
  </ul>
{%- endif -%}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=customer&type=Code}">`#customer`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= account&type=Code}">`#account`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= header&type=Code}">`#header`</a> 

------
### <a name="customer-login">Customer login</a>
A customer login form is used by visitors to log in to their customer account page. This example demonstrates the markup required. If creating an account is optional, display a **"Continue as Guest"** control. Use JavaScript to show/hide the password recovery form.

1.  Place the following code in the `Template/customers/login.liquid` file.

```liquid
<h1>Sign in to your Account</h1>

{%- form 'customer_login' -%}
  {{ form.errors | default_errors }}

  <div>
    <label for="customerEmail">Email Address</label>
    <input type="email"
      name="customer[email]"
      id="customerEmail"
      autocorrect="off"
      autocapitalize="off"
      autocomplete="email">
  </div>
  <div>
    <label for="customerPassword">Password</label>
    <input type="password"
      name="customer[password]"
      id="customerPassword">
  </div>

  <input type="submit" value="Sign In" />

  <p>
    {{ 'layout.customer.create_account' | t | customer_register_link }}
  </p>
  <p>
    <a href="#recover">{{ 'customer.login.forgot_password' | t }}</a>
  </p>

{%- endform -%}

<!-- If accounts are set as optional, the following will be shown as an option when coming from checkout, not on the default /login page. -->
{%- if shop.checkout.guest_login -%}
  {%- form 'guest_login' -%}
    <input type="submit" value="Continue as Guest" />
  {%- endform -%}
{%- endif -%}

<!-- Use JavaScript to show/hide this form -->
{%- form 'recover_customer_password' -%}

  {%- if form.posted_successfully? -%}
    <div role="status">
      {{ 'customer.recover_password.success' | t }}
    </div>
  {%- endif -%}

  <div id="recover"{% unless form.errors %} style="display: none;"{% endunless %}>
    <p>We will send you an email to reset your password.</p>

    {{ form.errors | default_errors }}

    <label for="customerEmail">Email Address</label>
    <input type="email"
      name="email"
      id="customerEmail"
      autocorrect="off"
      autocapitalize="off"
      autocomplete="email">

    <input type="submit" value="Submit">
  </div>

{%- endform -%}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=login&type=Code}">`#login`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= customer&type=Code}">`#customer`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= form&type=Code}">`#form`</a> 

------
### <a name="header-logo">Header logo</a>
A logo in the header of a website showcases the brand and usually also acts as a home navigation link. The purpose of wrapping with an `h1` only on the homepage is to ensure a top-level heading is available on that page. Subsequent landing pages should feature a unique, visible `h1` heading describing the page purpose, and therefore the logo only outputs an `h1` on the homepage of a store.

1.  Place the following code in within the `<header>` element or container of your theme, or wherever you wish the logo to appear.
2.  This code assumes you have a file called `logo.svg` in the `Assets` directory of your theme. Alternatively, you can replace this with a settings variable, define those settings in section `{% schema %}`, and allow a merchant to upload a logo image through the [theme editor](https://help.shopify.com/en/themes/development/theme-editor).
3.  `itemscope`,`itemtype` and `itemprop` are defined as per the [http://schema.org](https://schema.org/Organization) specification for organizations.

```liquid
{%- if template.name == 'index' -%}
  <h1 itemscope itemtype="http://schema.org/Organization">
{%- else -%}
  <div itemscope itemtype="http://schema.org/Organization">
{%- endif -%}

  <a href="/" itemprop="url">
    <img src="{{ "logo.svg" | asset_url }}"
      alt="{{ shop.name }}"
      itemprop="logo">
  </a>

{%- if template.name == 'index' -%}
  </h1>
{%- else -%}
  </div>
{%- endif -%}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=header&type=Code}">`#header`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= logo&type=Code}">`#logo`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= images&type=Code}">`#images`</a> 

------
### <a name="og-tags">OG tags</a>
OG tags/Twitter card allows a developer to control what content renders in a preview when a link is shared on Facebook, Twitter, or other platforms. This example demonstrates the markup required to share that metadata on social media.

1.  Place the following code in the `Snippets/social-meta-tags.liquid` file.
2.  Include the following in the `Layout/theme.liquid` file, directly above the closing `</head>` HTML tag.

```liquid
{%- assign og_title = page_title -%}
{%- assign og_url = canonical_url -%}
{%- assign og_type = 'website' -%}
{%- assign og_description = page_description | default: shop.description | default: shop.name -%}

{%- if settings.share_image -%}
  {%- capture og_image_tags -%}
    <meta property="og:image" content="http:{{ settings.share_image | img_url: '1200x1200' }}">
  {%- endcapture -%}

  {%- capture og_image_secure_url_tags -%}
    <meta property="og:image:secure_url" content="https:{{ settings.share_image | img_url: '1200x1200' }}">
  {%- endcapture -%}
{%- endif -%}

{%- case template.name -%}
  {%- when 'product' -%}
    {%- assign og_title = product.title | strip_html -%}
    {%- assign og_type = 'product' -%}

    {%- if product.images.size > 0 -%}
      {%- capture og_image_tags -%}
        {%- for image in product.images limit:3 -%}
          <meta property="og:image" content="http:{{ image.src | product_img_url: '1200x1200' }}">
        {%- endfor -%}
      {%- endcapture -%}

      {%- capture og_image_secure_url_tags -%}
        {%- for image in product.images limit:3 -%}
          <meta property="og:image:secure_url" content="https:{{ image.src | product_img_url: '1200x1200' }}">
        {%- endfor -%}
      {%- endcapture -%}
    {%- endif -%}

  {%- when 'article' -%}
    {%- assign og_title = article.title | strip_html -%}
    {%- assign og_type = 'article' -%}
    {%- assign og_description = article.excerpt_or_content | strip_html -%}

    {%- if article.image -%}
      {%- capture og_image_tags -%}
        <meta property="og:image" content="http:{{ article.image | img_url: '1200x1200' }}">
      {%- endcapture -%}

      {%- capture og_image_secure_url_tags -%}
        <meta property="og:image:secure_url" content="https:{{ article.image | img_url: '1200x1200' }}">
      {%- endcapture -%}
    {%- endif -%}

  {%- when 'collection' -%}
    {%- assign og_title = collection.title | strip_html -%}
    {%- assign og_type = 'product.group' -%}

    {%- if collection.image -%}
      {%- capture og_image_tags -%}
        <meta property="og:image" content="http:{{ collection.image | img_url: '1200x1200' }}">
      {%- endcapture -%}

      {%- capture og_image_secure_url_tags -%}
        <meta property="og:image:secure_url" content="https:{{ collection.image | img_url: '1200x1200' }}">
      {%- endcapture -%}
    {%- endif -%}

  {%- when 'password' -%}
    {%- assign og_title = shop.name -%}
    {%- assign og_url = shop.url -%}
    {%- assign og_description = shop.description | default: shop.name -%}

{%- endcase -%}

<meta property="og:site_name" content="{{ shop.name }}">
<meta property="og:url" content="{{ og_url }}">
<meta property="og:title" content="{{ og_title }}">
<meta property="og:type" content="{{ og_type }}">
<meta property="og:description" content="{{ og_description }}">

{%- if template.name == 'product' -%}
  <meta property="og:price:amount" content="{{ product.price | money_without_currency | strip_html }}">
  <meta property="og:price:currency" content="{{ shop.currency }}">
{%- endif -%}

{{ og_image_tags }}
{{ og_image_secure_url_tags }}

{%- unless settings.social_twitter_link == blank -%}
  <meta name="twitter:site" content="{{ settings.social_twitter_link | split: 'twitter.com/' | last | prepend: '@' }}">
{%- endunless -%}

<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:title" content="{{ og_title }}">
<meta name="twitter:description" content="{{ og_description }}">
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=header&type=Code}">`#header`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= images&type=Code}">`#images`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= socialmedia&type=Code}">`#socialmedia`</a> 

------
### <a name="password-page">Password page</a>
The password page is a landing page that adds password protection to your online store. Password pages are also sometimes used to collect email addresses before a store opens. This page is displayed after setting a store-wide password within the store preferences section of the Shopify admin.

1.  Place the following code in the `Layout/password.liquid` file.
2.  Enable store password protection by adding a password in the Shopify admin by navigating to **"Online Store"** > **"Preferences"**.

```liquid
<!doctype html>
<!--[if IE 9]> <html class="ie9 no-js" lang="{{ shop.locale }}"> <![endif]-->
<!--[if (gt IE 9)|!(IE)]><!--> <html class="no-js" lang="{{ shop.locale }}"> <!--<![endif]-->
<head>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <meta name="theme-color" content="{{ settings.color_primary }}">
  <link rel="canonical" href="{{ canonical_url }}">

  {%- if settings.favicon != blank -%}
    <link rel="shortcut icon" href="{{ settings.favicon | img_url: '32x32' }}" type="image/png">
  {%- endif -%}

  <title>{{ shop.name }}</title>

  {%- if page_description -%}
    <meta name="description" content="{{ page_description | escape }}">
  {%- endif -%}

  {%- include 'social-meta-tags' -%}

  {{ 'theme.scss.css' | asset_url | stylesheet_tag }}

  <!--[if (gt IE 9)|!(IE)]><!--><script src="{{ 'vendor.js' | asset_url }}" defer="defer"></script><!--<![endif]-->
  <!--[if lt IE 9]><script src="{{ 'vendor.js' | asset_url }}"></script><![endif]-->

  <!--[if (gt IE 9)|!(IE)]><!--><script src="{{ 'theme.js' | asset_url }}" defer="defer"></script><!--<![endif]-->
  <!--[if lt IE 9]><script src="{{ 'theme.js' | asset_url }}"></script><![endif]-->

  {{ content_for_header }}
</head>

<body>

  <header role="banner">
    <h1 itemscope itemtype="http://schema.org/Organization">
      {{ shop.name }}
    </h1>
  </header>

  <main role="main" id="MainContent">
    {{ content_for_layout }}
  </main>

  <footer role="complementary">
    {%- capture shopify_link -%}
      <a href="https://www.shopify.com" aria-label="Shopify">
        {%- include 'icon-shopify-logo' -%}
      </a>
    {%- endcapture -%}
    {{ 'general.password_page.powered_by_shopify_html' | t: shopify: shopify_link }}
  </footer>

  <div id="Login">
    <h2>{{ 'general.password_page.login_form_heading' | t }}</h2>

    {%- form 'storefront_password' -%}
      {{ form.errors | default_errors }}

      <label for="password">{{ 'general.password_page.login_form_password_label' | t }}</label>
      <input type="password"
        name="password"
        id="password"
        placeholder="{{ 'general.password_page.login_form_password_placeholder' | t }}">

      <button type="submit" name="commit">
        {{ 'general.password_page.login_form_submit' | t }}
      </button>
    {%- endform -%}

    <p>{{ 'general.password_page.admin_link_html' | t }}</p>
  </div>

</body>
</html>
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=password&type=Code}">`#password`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= page&type=Code}">`#page`</a> 

------
### <a name="payment-icons">Payment icons</a>
Payment icons show customers which payment methods are accepted by your online store. The icons are typically displayed in the footer of your website.

1.  Add the following code to the `footer.liquid` section file, or wherever you wish the icons to appear.

```liquid
<style>
.pmethod__item {
  display:inline;
}
.pmethod__icons {
  width: 50px;
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

{%- unless shop.enabled_payment_types == empty -%}
  <span class="visuallyhidden">Supported payment methods</span>
  <ul class="pmethod">
    {%- for type in shop.enabled_payment_types -%}
      <li class="pmethod__item">
        {{ type | payment_type_svg_tag: class: 'pmethod__icons' }}
      </li>
    {%- endfor -%}
  </ul>
{%- endunless -%}
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=payments #footer&type=Code}">`#payments #footer`</a> 

------
### <a name="skip-link">Skip link</a>
Skip links, also known as skip navigation links, are intended to make it easier for users navigating with a keyboard to skip over the main navigation and header elements of a site. It can be frustrating for users to have to repeatedly tab through navigation links to get to the main content of a page. Skip links solve this problem.

1.  Add the following link theme.liquid file, as the first child element inside the opening `<body>` tag. The href value should link to the id placed on the `<main>` content element.
2.  Skip links should always render before the navigation, logo, and main header elements of a site, as their purpose is to help sighted, keyboard-only users and screen reader users to skip to the main content quickly.

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

  .visuallyhidden.focusable:active,
  .visuallyhidden.focusable:focus {
    clip: auto;
    height: auto;
    margin: 0;
    overflow: visible;
    position: static;
    width: auto;
    white-space: inherit;
  }

  .skip-link {
    background-color: #fff;
    padding: 1em;
    z-index: 10000;
  }
</style>

<a class="visuallyhidden focusable skip-link" href="#main-content">Skip to content</a>

<!-- header / site navigation -->

<main id="main-content" role="main" tabindex="-1">
  {{ content_for_layout }}
</main>
```
<a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q=accessibility&type=Code}">`#accessibility`</a> <a href="https://github.com/Shopify/liquid-library-examples/search?l=Liquid&q= skip-link&type=Code}">`#skip-link`</a> 
