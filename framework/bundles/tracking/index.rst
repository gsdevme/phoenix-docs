.. _phoenix-bundle-tracking-index:

PhoenixTrackingBundle
---------------------

The `PhoenixTrackingBundle` provides a mechanism for inserting third party javascript snippets into templates. Snippets allow:

- Code reuse across projects
- Snippets to be enabled/disabled on a per environment basis
- Configuration to be provided for snippets (which can differ per environment)

Where possible we reccomend the use of Google Tag Manager or other third party javascript tag management for this purpose. However this is not always possible, and in any event, you may still need to implement javascript in order to send information to a data layer in a third party system.

Current Implementations
===============
The following snippets already have implementations and can be used out of the box, providing you implement the correct contexts in your templates. Each implementation has its own configuration reference:

- :ref:`phoenix-tracking-snippet-adform`	
- Affiliate Window
- Chango
- CrazyEgg
- Criteo
- Dotmailer
- Google Tag Manager
- MediaMind
- MicrosoftAdCenter
- Optimizely
- Peerius
- PollDaddy
- Quantcast
- Rakuten
- RavenJs
- RTBHouse
- Salecycle
- VeInteractive
- Vergic

Contexts
=======================

Tracking snippets are rendered by use of a common twig extension. This snippet declares 'blocks' or areas into which snippets will be rendered. It is up to you which areas to declare in your templates, but if using the built in snippets variations, the following contexts will be assumed to exist:

- Global
- Product
- Category
- Checkout
- Confirmation

Integration into templates
==========================
Tracking snippets are included by adding/enabling them via application configuration (config.yml). Within individal templates, these snippets are then rendered one by one into the correct area. Consider the following basic sample pages from shop.

Each of these templates shows usages of the twig function 'phoenix_tracking_snippets'. This function takes two arguments. The first is a string, or a collction of strings, representing the 'context' of that function within a page. The second (optional) argument allows data to be passed from the page, into tracking snippets (this is discouraged).

A base template used by all pages

.. code-block:: twig

    // app/Resources/views/base.html.twig

    <html>
    <head>
    	{% block head_js %}
        {{ phoenix_tracking_snippets('global_head_start') }}
        {% endblock head_js %}
    </head>
    <body>
        {% block footer_js %}
    	{{ phoenix_tracking_snippets('global') }}
    	{% endblock footer_js %}
    </body>
    </html>

And a product page, where we are overriding one of the twig blocks.

.. code-block:: twig

    // app/Resources/views/product.html.twig
    {% extends '::base.html.twig' %}
    {% block footer_js %}
    {{ phoenix_tracking_snippets(['global', 'product'], {product: product}) }}
    {% endblock footer_js %}

In each of these areas, depending on what snippets have been registered (as bundles), the javascript will render into the area matching the context. e.g If you have javascript that should be included on every page - use the 'global' context and ensure that every page on your site has a snippet with the 'global' context.

.. tip::
  This is generally achieved by having a sitewide base template which every page extends, in which the function 'phoenix_tracking_snippets' is used. As you can see above, if you override this block, make sure you include the 'global' context in addition to any extra contexts you are representing.

As you will see in the 'cookbook' entry, it is possible to request a snippet be rendered in multiple defined contexts (e.g product, category, homepage) or in all contexts *excluding* a defined context (e.g 'global' but not on the 'confirmation' page).

.. caution::
	There is nothing stopping you from using the same context twice on the same page. This would generally be a mistake and would result in your javascript snippet being rendered twice.

Cookbook / Implementing your own Snippet
========================================

Ensuring a snippet is only ever rendered once
=============================================

User specific variables & caching
=================================

