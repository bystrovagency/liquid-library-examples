# Sections 

 ## Table of contents
1. [Announcement bar](#announcement-bar) 
2. [Call to action](#call-to-action) 
3. [Featured text](#featured-text) 
4. [Homepage quotes](#homepage-quotes) 
5. [Logo list](#logo-list) 
6. [Password content](#password-content) 
 
 
------
### <a name="announcement-bar">Announcement bar</a>
An announcement bar allows merchants to display custom updates and promote discounts. When added to a theme, this static section can be displayed on the homepage or on all pages, and can be configured from the [theme editor](https://help.shopify.com/en/themes/development/theme-editor).

1.  Create a new file within the `sections` folder of your theme, and paste the code below into this file. Save as `announcement-bar.liquid`.
2.  On the `theme.liquid` file add `{% section 'announcement-bar' %}` in the position where you would like it to appear (eg. within the main page wrapper container, above the `{{ content_for_layout }}` Liquid object). More info on how static sections can be added to pages can be found on [our help docs.](https://help.shopify.com/en/themes/development/sections#static-and-dynamic-sections)
3.  Text, links, and the ability to enable the bar on the homepage or all pages can now be assigned from the [theme editor](https://help.shopify.com/en/themes/development/theme-editor).

```liquid
{%- if section.settings.show_announcement -%}
  {%- if section.settings.home_page_only == false or template.name == 'index' -%}

    {%- if section.settings.link == blank -%}
      <div>
    {%- else -%}
      <a href="{{ section.settings.link }}" >
    {%- endif -%}

      <p>{{ section.settings.text | escape }}</p>

    {%- if section.settings.link == blank -%}
      </div>
    {%- else -%}
      </a>
    {%- endif -%}

  {%- endif -%}
{%- endif -%}

{% schema %}
{
  "name": "Announcement bar",
  "settings": [
    {
      "type": "checkbox",
      "id": "show_announcement",
      "label": "Show announcement",
      "default": false
    },
    {
      "type": "checkbox",
      "id": "home_page_only",
      "label": "Home page only",
      "default": true
    },
    {
      "type": "text",
      "id": "text",
      "label": "Announcement text",
      "default": "Announce something here"
    },
    {
      "type": "url",
      "id": "link",
      "label": "Announcement link"
    }
  ]
}
{% endschema %}
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=global&type=Code}">`#global`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= promotional&type=Code}">`#promotional`</a> 

------
### <a name="call-to-action">Call to action</a>
Call-to-action buttons allow merchants to drive visitor engagement to specific products, or carry out a specific activity, such as creating an account or accessing content. This section will allow a [call-to-action button](https://www.shopify.com/partners/blog/building-a-clickable-call-to-action-button-for-your-shopify-theme) to be added to the home-page from the [theme editor](https://help.shopify.com/en/themes/development/theme-editor).

1.  Create a new file within the `sections` folder of your theme, and paste the code below into this file. Save file as `call-to-action.liquid`.
2.  Navigate to the Theme Editor and select **"Add section"**. Within the Promotional category there will be an option for **"Call to Action"**. A heading, link, and link text can be assigned in this section.

```liquid
<div id="section-cta">
  <h2>{{ section.settings.text-box }}</h2>

  <a href="{{ section.settings.link }}">
    {{ section.settings.linktext }}
  </a>
</div>

{% schema %}
{
  "name": "Call to action",
  "settings": [
    {
      "id": "text-box",
      "type": "text",
      "label": "Heading",
      "default": "Title"
    },
    {
      "id": "link",
      "type": "url",
      "label": "Link URL"
    },
    {
      "id": "linktext",
      "type": "text",
      "label": "Link text",
      "default": "Click here"
    }
  ]
  ,
  "presets": [
    {
      "name": "Call to Action",
      "category": "Promotional"
    }
  ]
}
{% endschema %}
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=home-page&type=Code}">`#home-page`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= promotional&type=Code}">`#promotional`</a> 

------
### <a name="featured-text">Featured text</a>
This dynamic section will create an option for featured text on a store's homepage. This allows merchants to add their own custom content or messaging in any position on the page.

1.  Create a new file within the `sections` folder of your theme, and paste the code below into this file. Name file `featured-text.liquid`.
2.  Navigate to the [theme editor](https://help.shopify.com/en/themes/development/theme-editor) and select **"Add section"**. Within the Text category, there will be an option for **"Rich Text"**. Custom content can be added here.

```liquid
<h2>{{ section.settings.section_title | escape }}</h2>
{{ section.settings.text }}

{% schema %}
{
  "name": "Rich text",
  "settings": [
    {
      "type": "text",
      "id": "section_title",
      "label": "Title",
      "default": "Talk about your brand"
    },
    {
      "type": "richtext",
      "id": "text",
      "label": "Text",
      "default": "<p>Use this text to share information about your brand with your customers. Describe a product, share announcements, or welcome customers to your store.</p>"
    }
  ],
  "presets": [
    {
      "name": "Rich Text",
      "category": "Text"
    }
  ]
}
{% endschema %}
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=home-page&type=Code}">`#home-page`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= text&type=Code}">`#text`</a> 

------
### <a name="homepage-quotes">Homepage quotes</a>
This section allows merchants to display quotes or testimonials from previous customers on their home-page. Quotes or testimonials are important because they allow merchants to build trust with customers by displaying positive messages from previous customers or brand supporters. This dynamic section makes use of section blocks to add multiple quotes, up to a maximum of 8.

1.  Create a new file within the `sections` folder of your theme, and paste the code below into this file. Save file as `quotes.liquid`.
2.  Add your own required styling for the quotes to the `theme.scss.liquid` file and include SVG quote icon if needed.
3.  Navigate to the [theme editor](https://help.shopify.com/en/themes/development/theme-editor) and select **"Add section"**. Within the "Text" category there will be an option for "Quotes". A maximum of 8 image blocks with links can be added.

```liquid
<h2>{{ section.settings.title }}</h2>

{%- if section.blocks.size > 0 -%}
  {%- for block in section.blocks -%}
    <blockquote>
      {%- if block.settings.quote != blank -%}
        <p>
          {{ block.settings.quote }}
        </p>
      {%- endif -%}
    </blockquote>

    {%- if block.settings.author != blank -%}
      <cite>
        <a href="{{ block.settings.link }}">
          {{ block.settings.author }}
        </a>
      </cite>
    {%- endif -%}
  {%- endfor -%}
{%- endif -%}

{% schema %}
{
  "name": "Testimonials",
  "class": "index-section",
  "max_blocks": 8,
  "settings": [
    {
      "type": "text",
      "id": "title",
      "label": "Heading",
      "default": "Testimonials"
    }
  ],
  "blocks": [
    {
      "type": "quote",
      "name": "Testimonial",
      "settings": [
        {
          "type": "richtext",
          "id": "quote",
          "label": "Text",
          "default": "<p>Add customer reviews and testimonials to showcase your store’s happy customers.</p>"
        },
        {
          "type": "text",
          "id": "author",
          "label": "Author",
          "default": "Author’s name"
        },
        {
    "type": "url",
          "id": "link",
          "label": "link"
        }
      ]
    }
  ],
  "presets": [
    {
      "name": "Testimonials",
      "category": "Text",
      "blocks": [
        {
          "type": "quote"
        },
        {
          "type": "quote"
        },
        {
          "type": "quote"
        }
      ]
    }
  ]
}
{% endschema %}
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=text&type=Code}">`#text`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= blocks&type=Code}">`#blocks`</a> 

------
### <a name="logo-list">Logo list</a>
The Logo list allows merchants to upload images of logos to display company sponsors or brands they have worked with and websites they have been featured on. Note: this code example uses the `placeholder_svg_tag` Liquid filter to output an SVG illustration when no logo is present in a block. You can learn more about the `placeholder_svg_tag` Liquid filter in [our Shopify Help Center.](https://help.shopify.com/en/themes/liquid/filters/additional-filters#placeholder_svg_tag)

1.  Create a new file within the `sections` folder of your theme, and paste the code below into this file. Save file as `logo-list.liquid`.
2.  Navigate to the [theme editor](https://help.shopify.com/en/themes/development/theme-editor) and select **"Add section"**. Within the Image category there will be an option for **"Logo list"**. A maximum of 10 image blocks with links can be added.
3.  Logo image widths can be adjusted from the theme editor. Different width parameters can be assigned to the `logo_width` settings.

```liquid
<style>
  .logo-bar__item {
    display: inline-block;
    max-width: {{ section.settings.logo_width }};
  }
</style>

<h2>{{ section.settings.title | escape }}</h2>

{%- if section.blocks.size > 0 -%}
  <ul>
    {%- for block in section.blocks -%}
      <li class="logo-bar__item" {{ block.shopify_attributes }}>
        {%- if block.settings.link != blank -%}
          <a href="{{ block.settings.link }}">
        {%- endif -%}

          {%- if block.settings.image != blank -%}
            {{ block.settings.image | img_url: '160x160', scale: 2 | img_tag: block.settings.image.alt }}
          {%- else -%}
            {{ 'logo' | placeholder_svg_tag: 'placeholder-svg' }}
          {%- endif -%}

        {%- if block.settings.link != blank -%}
          </a>
        {%- endif -%}
      </li>
    {%- endfor -%}
  </ul>
{%- endif -%}

{% schema %}
{
  "name": "Logo list",
  "class": "index-section",
  "max_blocks": 10,
  "settings": [
    {
      "type": "text",
      "id": "title",
      "label": "Heading",
      "default": "Logo list"
    },
    {
      "type": "select",
      "id": "logo_width",
      "label": "Logo width",
      "default": "160px",
      "options": [
        {
          "label": "Extra Small",
          "value": "100px"
        },
        {
          "label": "Small",
          "value": "125px"
        },
        {
          "label": "Medium",
          "value": "160px"
        },
        {
          "label": "Large",
          "value": "175px"
        },
        {
          "label": "Extra Large",
          "value": "200px"
        }
      ]
    }
  ],
  "blocks": [
    {
      "type": "logo_image",
      "name": "Logo",
      "settings": [
        {
          "type": "image_picker",
          "id": "image",
          "label": "Image"
        },
        {
          "type": "url",
          "id": "link",
          "label": "Link",
          "info": "Optional"
        }
      ]
    }
  ],
  "presets": [
    {
      "name": "Logo list",
      "category": "Image",
      "blocks": [
        {
          "type": "logo_image"
        },
        {
          "type": "logo_image"
        },
        {
          "type": "logo_image"
        },
        {
          "type": "logo_image"
        }
      ]
    }
  ]
}
{% endschema %}
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=images&type=Code}">`#images`</a> <a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q= blocks&type=Code}">`#blocks`</a> 

------
### <a name="password-content">Password content</a>
This section allows merchants to customize the content that appears on the password page. More info can be found on the [Shopify Web Design and Development Blog.](https://www.shopify.com/partners/blog/customize-shopify-password-pages-with-the-password-liquid-template)

1.  Create a new file within the `sections` folder of your theme, and paste the code below into this file. Save file as `password-content.liquid`.
2.  Where `<!--  code for newsletter form -->` appears, add your code for a newletter sign-up form.
3.  When the password page is enabled, you can navigate to the [theme editor](https://help.shopify.com/en/themes/development/theme-editor) and select **"Password page"** on the page selector drop-down. Heading, subheading, and placeholder text can be edited on this page.

```liquid
{%- unless shop.password_message == blank -%}
  {{ shop.password_message }}
{%- endunless -%}

{%- if section.settings.newsletter_enable -%}
  {%- form 'customer', id: 'ContactPassword' -%}
     <h2>{{ section.settings.newsletter_form_heading | escape }}</h2>

     {%- if section.settings.newsletter_form_subheading != blank -%}
       <p>{{ section.settings.newsletter_form_subheading }}</p>
     {%- endif -%}

     <!-- Code for newsletter form -->

  {%- endform -%}
{%- endif -%}

{% schema %}
{
  "name": "Content",
  "settings": [
    {
      "type": "checkbox",
      "id": "newsletter_enable",
      "label": "Newsletter Signup zeigen",
      "default": true
    },
    {
      "type": "text",
      "id": "newsletter_form_heading",
      "label": "Newsletter form heading",
      "default": "Be the first to know when we launch."
    },
    {
      "type": "richtext",
      "id": "newsletter_form_subheading",
      "label": "Subheading",
      "default": "<p>A short sentence describing what someone will receive by subscribing</p>"
    },
    {
      "type": "text",
      "id": "newsletter_placeholder",
      "label": "Newsletter placeholder text",
      "default": "Email address"
    },
    {
      "type": "text",
      "id": "newsletter_button_text",
      "label": "Newsletter button text",
      "default": "Notify me"
    }
  ]
}
{% endschema %}
```
<a href="https://github.com/Shopify/liquid-library/search?l=Liquid&q=password&type=Code}">`#password`</a> 
